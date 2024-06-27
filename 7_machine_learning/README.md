# Machine  Learning  Model  Training
## Steps  to  Train  and  Save  the  Model

1. **Load  Data  into  DataFrames** 
```python  
from  pyspark.sql  import  SparkSession  
from  pyspark.sql.types  import  StructType, StructField, IntegerType, StringType, DateType  

spark = SparkSession.builder.appName("Table Loading").getOrCreate() 
sc = spark.sparkContext 

# Load  Posts  dataframe  
posts = spark.read.parquet("/mnt/deBDProject/ml_training/Posts/*") 

# Load  PostTypes  dataframe  
PT_schema = StructType([ StructField("id", IntegerType(), True), StructField("Type", StringType(), True) ]) postType = spark.read.option("header", "true").option("sep", ",").schema(PT_schema).csv("/mnt/deBDProject/ml_training/PostTypes.txt") 

# Load  Users  dataframe  
users_schema = StructType([ 
	StructField("id", IntegerType(), True),
	StructField("Age", IntegerType(), True),
	StructField("CreationDate", DateType(), True),
	StructField("DisplayName", StringType(), True), 
	StructField("DownVotes", IntegerType(), True),
	StructField("EmailHash", StringType(), True),
	StructField("Location", StringType(), True),
	StructField("Reputation", IntegerType(), True),
	StructField("UpVotes", IntegerType(), True),
	StructField("Views", IntegerType(), True),
	StructField("WebsiteUrl", StringType(), True),
	StructField("AccountId", IntegerType(), True) ]) 
	
users = spark.read.option("header", "true").option("sep", ",").schema(users_schema).csv("/mnt/deBDProject/ml_training/users.csv") 

# Save  dataframes  for  easy  retrieval  
posts.write.parquet("/tmp/project/posts.parquet") postType.write.parquet("/tmp/project/PostType.parquet") users.write.parquet("/tmp/project/user.parquet")
```
2. **Join Tables and Filter Data**
```python
from pyspark.sql.functions import split, translate, trim, explode, regexp_replace, col, lower

# Load data
posts = spark.read.parquet("/tmp/project/posts.parquet")
postType = spark.read.parquet("/tmp/project/PostType.parquet")
users = spark.read.parquet("/tmp/project/user.parquet")

# Join Posts and PostTypes tables
df = posts.join(postType, posts.PostTypeId == postType.id)

# Filter to include only questions
df = df.filter(col("Type") == "Question")

# Format the 'Body' and 'Tags' columns for machine learning training
df = df.withColumn('Body', regexp_replace(df.Body, r'<.*?>', '')).withColumn("Tags", split(trim(translate(col("Tags"), "<>", " ")), " "))

# Create a checkpoint to save the dataframe with necessary columns
df = df.select(col("Body").alias("text"), col("Tags")).select("text", explode("Tags").alias("tags"))

# Save the dataframe to file
df.write.parquet("/tmp/project/df.parquet")
df.cache()
df.count()
```

3.  **Prepare Data for Machine Learning**
```py
# Clean the text data
cleaned = df.withColumn('text', regexp_replace('text', r"http\S+", "")) \
                    .withColumn('text', regexp_replace('text', r"[^a-zA-z]", " ")) \
                    .withColumn('text', regexp_replace('text', r"\s+", " ")) \
                    .withColumn('text', lower('text')) \
                    .withColumn('text', trim('text'))
```

4.  **Train the Machine Learning Model**
```py
from pyspark.ml.feature import Tokenizer, StopWordsRemover, CountVectorizer, IDF, StringIndexer
from pyspark.ml.classification import LogisticRegression
from pyspark.ml import Pipeline
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

# Train-test split
train, test = cleaned.randomSplit([0.9, 0.1], seed=20200819)

# Initialize transformers
tokenizer = Tokenizer(inputCol="text", outputCol="tokens")
stopword_remover = StopWordsRemover(inputCol="tokens", outputCol="filtered")
cv = CountVectorizer(vocabSize=2**16, inputCol="filtered", outputCol='cv')
idf = IDF(inputCol='cv', outputCol="features", minDocFreq=5)
label_encoder = StringIndexer(inputCol = "tags", outputCol = "label")
lr = LogisticRegression(maxIter=100)

# Create and train pipeline
pipeline = Pipeline(stages=[tokenizer, stopword_remover, cv, idf, label_encoder, lr])
pipeline_model = pipeline.fit(train)
predictions = pipeline_model.transform(test)

# Evaluate the model
evaluator = MulticlassClassificationEvaluator(predictionCol="prediction")
roc_auc = evaluator.evaluate(predictions)
accuracy = predictions.filter(predictions.label == predictions.prediction).count() / float(predictions.count())

print("Accuracy Score: {0:.4f}".format(accuracy))
print("ROC-AUC: {0:.4f}".format(roc_auc))
```

5.  **Save the Model**
```py
# Save model and label encoder
pipeline_model.save('/mnt/deBDProject/model')
le_model.save('/mnt/deBDProject/stringindexer')

# Validate saved model
display(dbutils.fs.ls("/mnt/deBDProject/model"))
```