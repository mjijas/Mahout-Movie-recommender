# Mahout-Movie-recommender
# this file contains terminal command for more details 
ijaz@ijaz-HP-ENVY-15-Notebook-PC:~$ cd Documents/AWS
ijaz@ijaz-HP-ENVY-15-Notebook-PC:~/Documents/AWS$ dir
ijazKeyPair.pem
ijaz@ijaz-HP-ENVY-15-Notebook-PC:~/Documents/AWS$ ssh -i ijazKeyPair.pem hadoop@ec2-34-202-229-11.compute-1.amazonaws.com
The authenticity of host 'ec2-34-202-229-11.compute-1.amazonaws.com (34.202.229.11)' can't be established.
ECDSA key fingerprint is SHA256:w09h3rFsCJWYZgjp6tcMHAyy5cJUvkTb/VPdQ3ci+Xs.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ec2-34-202-229-11.compute-1.amazonaws.com,34.202.229.11' (ECDSA) to the list of known hosts.
Last login: Sun Mar  1 15:09:31 2020

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
18 package(s) needed for security, out of 33 available
Run "sudo yum update" to apply all updates.
                                                                    
EEEEEEEEEEEEEEEEEEEE MMMMMMMM           MMMMMMMM RRRRRRRRRRRRRRR    
E::::::::::::::::::E M:::::::M         M:::::::M R::::::::::::::R   
EE:::::EEEEEEEEE:::E M::::::::M       M::::::::M R:::::RRRRRR:::::R 
  E::::E       EEEEE M:::::::::M     M:::::::::M RR::::R      R::::R
  E::::E             M::::::M:::M   M:::M::::::M   R:::R      R::::R
  E:::::EEEEEEEEEE   M:::::M M:::M M:::M M:::::M   R:::RRRRRR:::::R 
  E::::::::::::::E   M:::::M  M:::M:::M  M:::::M   R:::::::::::RR   
  E:::::EEEEEEEEEE   M:::::M   M:::::M   M:::::M   R:::RRRRRR::::R  
  E::::E             M:::::M    M:::M    M:::::M   R:::R      R::::R
  E::::E       EEEEE M:::::M     MMM     M:::::M   R:::R      R::::R
EE:::::EEEEEEEE::::E M:::::M             M:::::M   R:::R      R::::R
E::::::::::::::::::E M:::::M             M:::::M RR::::R      R::::R
EEEEEEEEEEEEEEEEEEEE MMMMMMM             MMMMMMM RRRRRRR      RRRRRR
                                                                    
[hadoop@ip-172-31-19-194 ~]$ wget http://files.grouplens.org/datasets/movielens/ml-1m.zip
--2020-03-01 15:12:29--  http://files.grouplens.org/datasets/movielens/ml-1m.zip
Resolving files.grouplens.org (files.grouplens.org)... 128.101.65.152
Connecting to files.grouplens.org (files.grouplens.org)|128.101.65.152|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5917549 (5.6M) [application/zip]
Saving to: ‘ml-1m.zip’

ml-1m.zip                                      100%[===================================================================================================>]   5.64M  23.2MB/s    in 0.2s    

2020-03-01 15:12:29 (23.2 MB/s) - ‘ml-1m.zip’ saved [5917549/5917549]

[hadoop@ip-172-31-19-194 ~]$ unzip ml-1m.zip
Archive:  ml-1m.zip
   creating: ml-1m/
  inflating: ml-1m/movies.dat        
  inflating: ml-1m/ratings.dat       
  inflating: ml-1m/README            
  inflating: ml-1m/users.dat         
[hadoop@ip-172-31-19-194 ~]$ cat ml-1m/ratings.dat | sed 's/::/,/g' | cut -f1-3 -d, > ratings.csv
[hadoop@ip-172-31-19-194 ~]$ hadoop fs -put ratings.csv /ratings.csv
[hadoop@ip-172-31-19-194 ~]$ mahout recommenditembased --input /ratings.csv --output recommendations --numRecommendations 10 --outputPathForSimilarityMatrix similarity-matrix --similarityClassname SIMILARITY_COSINE
MAHOUT_LOCAL is not set; adding HADOOP_CONF_DIR to classpath.
Running on hadoop, using /usr/lib/hadoop/bin/hadoop and HADOOP_CONF_DIR=/etc/hadoop/conf
MAHOUT-JOB: /usr/lib/mahout/mahout-examples-0.13.0-job.jar
20/03/01 15:13:18 INFO AbstractJob: Command line arguments: {--booleanData=[false], --endPhase=[2147483647], --input=[/ratings.csv], --maxPrefsInItemSimilarity=[500], --maxPrefsPerUser=[10], --maxSimilaritiesPerItem=[100], --minPrefsPerUser=[1], --numRecommendations=[10], --output=[recommendations], --outputPathForSimilarityMatrix=[similarity-matrix], --similarityClassname=[SIMILARITY_COSINE], --startPhase=[0], --tempDir=[temp]}
20/03/01 15:13:18 INFO AbstractJob: Command line arguments: {--booleanData=[false], --endPhase=[2147483647], --input=[/ratings.csv], --minPrefsPerUser=[1], --output=[temp/preparePreferenceMatrix], --ratingShift=[0.0], --startPhase=[0], --tempDir=[temp]}
20/03/01 15:13:19 INFO deprecation: mapred.input.dir is deprecated. Instead, use mapreduce.input.fileinputformat.inputdir
20/03/01 15:13:19 INFO deprecation: mapred.compress.map.output is deprecated. Instead, use mapreduce.map.output.compress
20/03/01 15:13:19 INFO deprecation: mapred.output.dir is deprecated. Instead, use mapreduce.output.fileoutputformat.outputdir
20/03/01 15:13:19 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:13:20 INFO FileInputFormat: Total input files to process : 1
20/03/01 15:13:20 INFO GPLNativeCodeLoader: Loaded native gpl library
20/03/01 15:13:20 INFO LzoCodec: Successfully loaded & initialized native-lzo library [hadoop-lzo rev 5f788d5e8f90539ee331702c753fa250727128f4]
20/03/01 15:13:20 INFO JobSubmitter: number of splits:1
20/03/01 15:13:20 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0001
20/03/01 15:13:20 INFO YarnClientImpl: Submitted application application_1583075263102_0001
20/03/01 15:13:21 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0001/
20/03/01 15:13:21 INFO Job: Running job: job_1583075263102_0001
20/03/01 15:13:28 INFO Job: Job job_1583075263102_0001 running in uber mode : false
20/03/01 15:13:28 INFO Job:  map 0% reduce 0%
20/03/01 15:13:36 INFO Job:  map 100% reduce 0%
20/03/01 15:13:41 INFO Job:  map 100% reduce 100%
20/03/01 15:13:41 INFO Job: Job job_1583075263102_0001 completed successfully
20/03/01 15:13:41 INFO Job: Counters: 49
	File System Counters
		FILE: Number of bytes read=22181
		FILE: Number of bytes written=725173
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=11553574
		HDFS: Number of bytes written=45077
		HDFS: Number of read operations=12
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Launched map tasks=1
		Launched reduce tasks=3
		Data-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=522912
		Total time spent by all reduces in occupied slots (ms)=1830528
		Total time spent by all map tasks (ms)=5447
		Total time spent by all reduce tasks (ms)=9534
		Total vcore-milliseconds taken by all map tasks=5447
		Total vcore-milliseconds taken by all reduce tasks=9534
		Total megabyte-milliseconds taken by all map tasks=16733184
		Total megabyte-milliseconds taken by all reduce tasks=58576896
	Map-Reduce Framework
		Map input records=1000209
		Map output records=1000209
		Map output bytes=3946230
		Map output materialized bytes=22169
		Input split bytes=118
		Combine input records=1000209
		Combine output records=3706
		Reduce input groups=3706
		Reduce shuffle bytes=22169
		Reduce input records=3706
		Reduce output records=3706
		Spilled Records=7412
		Shuffled Maps =3
		Failed Shuffles=0
		Merged Map outputs=3
		GC time elapsed (ms)=293
		CPU time spent (ms)=6730
		Physical memory (bytes) snapshot=1374158848
		Virtual memory (bytes) snapshot=26515705856
		Total committed heap usage (bytes)=1273495552
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=11553456
	File Output Format Counters 
		Bytes Written=45077
20/03/01 15:13:41 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:13:41 INFO FileInputFormat: Total input files to process : 1
20/03/01 15:13:41 INFO JobSubmitter: number of splits:1
20/03/01 15:13:41 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0002
20/03/01 15:13:41 INFO YarnClientImpl: Submitted application application_1583075263102_0002
20/03/01 15:13:41 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0002/
20/03/01 15:13:41 INFO Job: Running job: job_1583075263102_0002
20/03/01 15:13:47 INFO Job: Job job_1583075263102_0002 running in uber mode : false
20/03/01 15:13:47 INFO Job:  map 0% reduce 0%
20/03/01 15:13:54 INFO Job:  map 100% reduce 0%
20/03/01 15:14:00 INFO Job:  map 100% reduce 33%
20/03/01 15:14:01 INFO Job:  map 100% reduce 100%
20/03/01 15:14:01 INFO Job: Job job_1583075263102_0002 completed successfully
20/03/01 15:14:01 INFO Job: Counters: 51
	File System Counters
		FILE: Number of bytes read=5275969
		FILE: Number of bytes written=11234169
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=11553574
		HDFS: Number of bytes written=6105967
		HDFS: Number of read operations=12
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed reduce tasks=1
		Launched map tasks=1
		Launched reduce tasks=3
		Rack-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=487488
		Total time spent by all reduces in occupied slots (ms)=2163072
		Total time spent by all map tasks (ms)=5078
		Total time spent by all reduce tasks (ms)=11266
		Total vcore-milliseconds taken by all map tasks=5078
		Total vcore-milliseconds taken by all reduce tasks=11266
		Total megabyte-milliseconds taken by all map tasks=15599616
		Total megabyte-milliseconds taken by all reduce tasks=69218304
	Map-Reduce Framework
		Map input records=1000209
		Map output records=1000209
		Map output bytes=7964758
		Map output materialized bytes=5275957
		Input split bytes=118
		Combine input records=0
		Combine output records=0
		Reduce input groups=6040
		Reduce shuffle bytes=5275957
		Reduce input records=1000209
		Reduce output records=6040
		Spilled Records=2000418
		Shuffled Maps =3
		Failed Shuffles=0
		Merged Map outputs=3
		GC time elapsed (ms)=312
		CPU time spent (ms)=9850
		Physical memory (bytes) snapshot=1406201856
		Virtual memory (bytes) snapshot=26489389056
		Total committed heap usage (bytes)=1363148800
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=11553456
	File Output Format Counters 
		Bytes Written=6105967
	org.apache.mahout.cf.taste.hadoop.item.ToUserVectorsReducer$Counters
		USERS=6040
20/03/01 15:14:01 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:14:02 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:14:02 INFO JobSubmitter: number of splits:3
20/03/01 15:14:02 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0003
20/03/01 15:14:02 INFO YarnClientImpl: Submitted application application_1583075263102_0003
20/03/01 15:14:02 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0003/
20/03/01 15:14:02 INFO Job: Running job: job_1583075263102_0003
20/03/01 15:14:07 INFO Job: Job job_1583075263102_0003 running in uber mode : false
20/03/01 15:14:07 INFO Job:  map 0% reduce 0%
20/03/01 15:14:14 INFO Job:  map 67% reduce 0%
20/03/01 15:14:15 INFO Job:  map 100% reduce 0%
20/03/01 15:14:20 INFO Job:  map 100% reduce 33%
20/03/01 15:14:21 INFO Job:  map 100% reduce 100%
20/03/01 15:14:21 INFO Job: Job job_1583075263102_0003 completed successfully
20/03/01 15:14:21 INFO Job: Counters: 50
	File System Counters
		FILE: Number of bytes read=4066525
		FILE: Number of bytes written=8908844
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=6106483
		HDFS: Number of bytes written=6086175
		HDFS: Number of read operations=21
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed map tasks=1
		Launched map tasks=3
		Launched reduce tasks=3
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=1338144
		Total time spent by all reduces in occupied slots (ms)=1935552
		Total time spent by all map tasks (ms)=13939
		Total time spent by all reduce tasks (ms)=10081
		Total vcore-milliseconds taken by all map tasks=13939
		Total vcore-milliseconds taken by all reduce tasks=10081
		Total megabyte-milliseconds taken by all map tasks=42820608
		Total megabyte-milliseconds taken by all reduce tasks=61937664
	Map-Reduce Framework
		Map input records=6040
		Map output records=1000209
		Map output bytes=16987356
		Map output materialized bytes=3820609
		Input split bytes=516
		Combine input records=1000209
		Combine output records=10660
		Reduce input groups=3706
		Reduce shuffle bytes=3820609
		Reduce input records=10660
		Reduce output records=3706
		Spilled Records=21320
		Shuffled Maps =9
		Failed Shuffles=0
		Merged Map outputs=9
		GC time elapsed (ms)=502
		CPU time spent (ms)=16730
		Physical memory (bytes) snapshot=2547699712
		Virtual memory (bytes) snapshot=35786366976
		Total committed heap usage (bytes)=2360344576
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=6105967
	File Output Format Counters 
		Bytes Written=6086175
20/03/01 15:14:21 INFO AbstractJob: Command line arguments: {--endPhase=[2147483647], --excludeSelfSimilarity=[true], --input=[temp/preparePreferenceMatrix/ratingMatrix], --maxObservationsPerColumn=[500], --maxObservationsPerRow=[500], --maxSimilaritiesPerRow=[100], --numberOfColumns=[6040], --output=[temp/similarityMatrix], --randomSeed=[-9223372036854775808], --similarityClassname=[SIMILARITY_COSINE], --startPhase=[0], --tempDir=[temp], --threshold=[4.9E-324]}
20/03/01 15:14:21 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:14:21 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:14:21 INFO JobSubmitter: number of splits:3
20/03/01 15:14:21 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0004
20/03/01 15:14:21 INFO YarnClientImpl: Submitted application application_1583075263102_0004
20/03/01 15:14:21 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0004/
20/03/01 15:14:21 INFO Job: Running job: job_1583075263102_0004
20/03/01 15:14:27 INFO Job: Job job_1583075263102_0004 running in uber mode : false
20/03/01 15:14:27 INFO Job:  map 0% reduce 0%
20/03/01 15:14:32 INFO Job:  map 33% reduce 0%
20/03/01 15:14:33 INFO Job:  map 100% reduce 0%
20/03/01 15:14:36 INFO Job:  map 100% reduce 100%
20/03/01 15:14:36 INFO Job: Job job_1583075263102_0004 completed successfully
20/03/01 15:14:36 INFO Job: Counters: 50
	File System Counters
		FILE: Number of bytes read=97123
		FILE: Number of bytes written=876489
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=6086694
		HDFS: Number of bytes written=60379
		HDFS: Number of read operations=15
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=3
	Job Counters 
		Killed map tasks=1
		Launched map tasks=3
		Launched reduce tasks=1
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=979584
		Total time spent by all reduces in occupied slots (ms)=433920
		Total time spent by all map tasks (ms)=10204
		Total time spent by all reduce tasks (ms)=2260
		Total vcore-milliseconds taken by all map tasks=10204
		Total vcore-milliseconds taken by all reduce tasks=2260
		Total megabyte-milliseconds taken by all map tasks=31346688
		Total megabyte-milliseconds taken by all reduce tasks=13885440
	Map-Reduce Framework
		Map input records=3706
		Map output records=3
		Map output bytes=180843
		Map output materialized bytes=97181
		Input split bytes=519
		Combine input records=3
		Combine output records=3
		Reduce input groups=1
		Reduce shuffle bytes=97181
		Reduce input records=3
		Reduce output records=0
		Spilled Records=6
		Shuffled Maps =3
		Failed Shuffles=0
		Merged Map outputs=3
		GC time elapsed (ms)=297
		CPU time spent (ms)=4390
		Physical memory (bytes) snapshot=1800204288
		Virtual memory (bytes) snapshot=21107036160
		Total committed heap usage (bytes)=1680343040
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=6086175
	File Output Format Counters 
		Bytes Written=98
20/03/01 15:14:36 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:14:37 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:14:37 INFO JobSubmitter: number of splits:3
20/03/01 15:14:37 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0005
20/03/01 15:14:37 INFO YarnClientImpl: Submitted application application_1583075263102_0005
20/03/01 15:14:37 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0005/
20/03/01 15:14:37 INFO Job: Running job: job_1583075263102_0005
20/03/01 15:14:43 INFO Job: Job job_1583075263102_0005 running in uber mode : false
20/03/01 15:14:43 INFO Job:  map 0% reduce 0%
20/03/01 15:14:48 INFO Job:  map 33% reduce 0%
20/03/01 15:14:50 INFO Job:  map 100% reduce 0%
20/03/01 15:14:53 INFO Job:  map 100% reduce 33%
20/03/01 15:14:54 INFO Job:  map 100% reduce 67%
20/03/01 15:14:55 INFO Job:  map 100% reduce 100%
20/03/01 15:14:55 INFO Job: Job job_1583075263102_0005 completed successfully
20/03/01 15:14:55 INFO Job: Counters: 53
	File System Counters
		FILE: Number of bytes read=6038829
		FILE: Number of bytes written=12054515
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=6267537
		HDFS: Number of bytes written=6692135
		HDFS: Number of read operations=24
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=9
	Job Counters 
		Killed map tasks=1
		Launched map tasks=3
		Launched reduce tasks=3
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=1355328
		Total time spent by all reduces in occupied slots (ms)=1693824
		Total time spent by all map tasks (ms)=14118
		Total time spent by all reduce tasks (ms)=8822
		Total vcore-milliseconds taken by all map tasks=14118
		Total vcore-milliseconds taken by all reduce tasks=8822
		Total megabyte-milliseconds taken by all map tasks=43370496
		Total megabyte-milliseconds taken by all reduce tasks=54202368
	Map-Reduce Framework
		Map input records=3706
		Map output records=655781
		Map output bytes=13748028
		Map output materialized bytes=4980549
		Input split bytes=519
		Combine input records=655781
		Combine output records=18109
		Reduce input groups=6043
		Reduce shuffle bytes=4980549
		Reduce input records=18109
		Reduce output records=6040
		Spilled Records=36218
		Shuffled Maps =9
		Failed Shuffles=0
		Merged Map outputs=9
		GC time elapsed (ms)=521
		CPU time spent (ms)=16030
		Physical memory (bytes) snapshot=2475118592
		Virtual memory (bytes) snapshot=35708821504
		Total committed heap usage (bytes)=2377121792
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=6086175
	File Output Format Counters 
		Bytes Written=6692114
	org.apache.mahout.math.hadoop.similarity.cooccurrence.RowSimilarityJob$Counters
		NEGLECTED_OBSERVATIONS=344437
		ROWS=3706
		USED_OBSERVATIONS=655772
20/03/01 15:14:55 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:14:55 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:14:56 INFO JobSubmitter: number of splits:3
20/03/01 15:14:56 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0006
20/03/01 15:14:56 INFO YarnClientImpl: Submitted application application_1583075263102_0006
20/03/01 15:14:56 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0006/
20/03/01 15:14:56 INFO Job: Running job: job_1583075263102_0006
20/03/01 15:15:01 INFO Job: Job job_1583075263102_0006 running in uber mode : false
20/03/01 15:15:01 INFO Job:  map 0% reduce 0%
20/03/01 15:15:12 INFO Job:  map 33% reduce 0%
20/03/01 15:15:15 INFO Job:  map 100% reduce 0%
20/03/01 15:15:20 INFO Job:  map 100% reduce 100%
20/03/01 15:15:20 INFO Job: Job job_1583075263102_0006 completed successfully
20/03/01 15:15:20 INFO Job: Counters: 52
	File System Counters
		FILE: Number of bytes read=367391417
		FILE: Number of bytes written=551520907
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=6692609
		HDFS: Number of bytes written=48566549
		HDFS: Number of read operations=30
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed map tasks=1
		Launched map tasks=3
		Launched reduce tasks=3
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=3080640
		Total time spent by all reduces in occupied slots (ms)=2928768
		Total time spent by all map tasks (ms)=32090
		Total time spent by all reduce tasks (ms)=15254
		Total vcore-milliseconds taken by all map tasks=32090
		Total vcore-milliseconds taken by all reduce tasks=15254
		Total megabyte-milliseconds taken by all map tasks=98580480
		Total megabyte-milliseconds taken by all reduce tasks=93720576
	Map-Reduce Framework
		Map input records=6040
		Map output records=655772
		Map output bytes=777392691
		Map output materialized bytes=183603602
		Input split bytes=432
		Combine input records=655772
		Combine output records=20223
		Reduce input groups=3678
		Reduce shuffle bytes=183603602
		Reduce input records=20223
		Reduce output records=3678
		Spilled Records=60669
		Shuffled Maps =9
		Failed Shuffles=0
		Merged Map outputs=9
		GC time elapsed (ms)=993
		CPU time spent (ms)=51970
		Physical memory (bytes) snapshot=3982159872
		Virtual memory (bytes) snapshot=35853176832
		Total committed heap usage (bytes)=3973578752
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=6692114
	File Output Format Counters 
		Bytes Written=48566549
	org.apache.mahout.math.hadoop.similarity.cooccurrence.RowSimilarityJob$Counters
		COOCCURRENCES=77007533
		PRUNED_COOCCURRENCES=0
20/03/01 15:15:20 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:15:21 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:15:21 INFO JobSubmitter: number of splits:3
20/03/01 15:15:21 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0007
20/03/01 15:15:21 INFO YarnClientImpl: Submitted application application_1583075263102_0007
20/03/01 15:15:21 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0007/
20/03/01 15:15:21 INFO Job: Running job: job_1583075263102_0007
20/03/01 15:15:27 INFO Job: Job job_1583075263102_0007 running in uber mode : false
20/03/01 15:15:27 INFO Job:  map 0% reduce 0%
20/03/01 15:15:35 INFO Job:  map 33% reduce 0%
20/03/01 15:15:36 INFO Job:  map 67% reduce 0%
20/03/01 15:15:37 INFO Job:  map 100% reduce 0%
20/03/01 15:15:41 INFO Job:  map 100% reduce 67%
20/03/01 15:15:42 INFO Job:  map 100% reduce 100%
20/03/01 15:15:42 INFO Job: Job job_1583075263102_0007 completed successfully
20/03/01 15:15:43 INFO Job: Counters: 50
	File System Counters
		FILE: Number of bytes read=10439779
		FILE: Number of bytes written=21910065
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=48567014
		HDFS: Number of bytes written=3747009
		HDFS: Number of read operations=21
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed map tasks=1
		Launched map tasks=3
		Launched reduce tasks=3
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=2017056
		Total time spent by all reduces in occupied slots (ms)=1731840
		Total time spent by all map tasks (ms)=21011
		Total time spent by all reduce tasks (ms)=9020
		Total vcore-milliseconds taken by all map tasks=21011
		Total vcore-milliseconds taken by all reduce tasks=9020
		Total megabyte-milliseconds taken by all map tasks=64545792
		Total megabyte-milliseconds taken by all reduce tasks=55418880
	Map-Reduce Framework
		Map input records=3678
		Map output records=4846890
		Map output bytes=104963593
		Map output materialized bytes=10446911
		Input split bytes=465
		Combine input records=4846890
		Combine output records=11022
		Reduce input groups=3678
		Reduce shuffle bytes=10446911
		Reduce input records=11022
		Reduce output records=3678
		Spilled Records=22044
		Shuffled Maps =9
		Failed Shuffles=0
		Merged Map outputs=9
		GC time elapsed (ms)=548
		CPU time spent (ms)=26820
		Physical memory (bytes) snapshot=2818646016
		Virtual memory (bytes) snapshot=35767500800
		Total committed heap usage (bytes)=2877292544
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=48566549
	File Output Format Counters 
		Bytes Written=3747009
20/03/01 15:15:43 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:15:43 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:15:43 INFO JobSubmitter: number of splits:3
20/03/01 15:15:43 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0008
20/03/01 15:15:43 INFO YarnClientImpl: Submitted application application_1583075263102_0008
20/03/01 15:15:43 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0008/
20/03/01 15:15:43 INFO Job: Running job: job_1583075263102_0008
20/03/01 15:15:48 INFO Job: Job job_1583075263102_0008 running in uber mode : false
20/03/01 15:15:48 INFO Job:  map 0% reduce 0%
20/03/01 15:15:54 INFO Job:  map 33% reduce 0%
20/03/01 15:15:55 INFO Job:  map 100% reduce 0%
20/03/01 15:15:59 INFO Job:  map 100% reduce 33%
20/03/01 15:16:01 INFO Job:  map 100% reduce 67%
20/03/01 15:16:02 INFO Job:  map 100% reduce 100%
20/03/01 15:16:02 INFO Job: Job job_1583075263102_0008 completed successfully
20/03/01 15:16:02 INFO Job: Counters: 50
	File System Counters
		FILE: Number of bytes read=3725519
		FILE: Number of bytes written=9117683
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=3882699
		HDFS: Number of bytes written=7983705
		HDFS: Number of read operations=42
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed map tasks=1
		Launched map tasks=3
		Launched reduce tasks=3
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=1257888
		Total time spent by all reduces in occupied slots (ms)=2355840
		Total time spent by all map tasks (ms)=13103
		Total time spent by all reduce tasks (ms)=12270
		Total vcore-milliseconds taken by all map tasks=13103
		Total vcore-milliseconds taken by all reduce tasks=12270
		Total megabyte-milliseconds taken by all map tasks=40252416
		Total megabyte-milliseconds taken by all reduce tasks=75386880
	Map-Reduce Framework
		Map input records=3678
		Map output records=365272
		Map output bytes=4369910
		Map output materialized bytes=4368669
		Input split bytes=459
		Combine input records=0
		Combine output records=0
		Reduce input groups=274065
		Reduce shuffle bytes=4368669
		Reduce input records=365272
		Reduce output records=274065
		Spilled Records=730544
		Shuffled Maps =9
		Failed Shuffles=0
		Merged Map outputs=9
		GC time elapsed (ms)=507
		CPU time spent (ms)=17450
		Physical memory (bytes) snapshot=2418159616
		Virtual memory (bytes) snapshot=35757199360
		Total committed heap usage (bytes)=2330984448
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=3747009
	File Output Format Counters 
		Bytes Written=7983705
20/03/01 15:16:02 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:16:03 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:16:03 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:16:03 INFO JobSubmitter: number of splits:6
20/03/01 15:16:03 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0009
20/03/01 15:16:03 INFO YarnClientImpl: Submitted application application_1583075263102_0009
20/03/01 15:16:03 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0009/
20/03/01 15:16:03 INFO Job: Running job: job_1583075263102_0009
20/03/01 15:16:08 INFO Job: Job job_1583075263102_0009 running in uber mode : false
20/03/01 15:16:08 INFO Job:  map 0% reduce 0%
20/03/01 15:16:16 INFO Job:  map 33% reduce 0%
20/03/01 15:16:17 INFO Job:  map 50% reduce 0%
20/03/01 15:16:18 INFO Job:  map 83% reduce 0%
20/03/01 15:16:19 INFO Job:  map 100% reduce 0%
20/03/01 15:16:21 INFO Job:  map 100% reduce 33%
20/03/01 15:16:23 INFO Job:  map 100% reduce 67%
20/03/01 15:16:25 INFO Job:  map 100% reduce 100%
20/03/01 15:16:25 INFO Job: Job job_1583075263102_0009 completed successfully
20/03/01 15:16:25 INFO Job: Counters: 51
	File System Counters
		FILE: Number of bytes read=6998790
		FILE: Number of bytes written=15742620
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=9855061
		HDFS: Number of bytes written=8301337
		HDFS: Number of read operations=33
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed map tasks=1
		Launched map tasks=6
		Launched reduce tasks=3
		Data-local map tasks=4
		Rack-local map tasks=2
		Total time spent by all maps in occupied slots (ms)=3801888
		Total time spent by all reduces in occupied slots (ms)=2073408
		Total time spent by all map tasks (ms)=39603
		Total time spent by all reduce tasks (ms)=10799
		Total vcore-milliseconds taken by all map tasks=39603
		Total vcore-milliseconds taken by all reduce tasks=10799
		Total megabyte-milliseconds taken by all map tasks=121660416
		Total megabyte-milliseconds taken by all reduce tasks=66349056
	Map-Reduce Framework
		Map input records=9718
		Map output records=1003887
		Map output bytes=11203078
		Map output materialized bytes=7208499
		Input split bytes=2085
		Combine input records=0
		Combine output records=0
		Reduce input groups=3706
		Reduce shuffle bytes=7208499
		Reduce input records=1003887
		Reduce output records=3678
		Spilled Records=2007774
		Shuffled Maps =18
		Failed Shuffles=0
		Merged Map outputs=18
		GC time elapsed (ms)=1062
		CPU time spent (ms)=24260
		Physical memory (bytes) snapshot=3857629184
		Virtual memory (bytes) snapshot=49633976320
		Total committed heap usage (bytes)=3584557056
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=8301337
20/03/01 15:16:25 INFO deprecation: io.sort.factor is deprecated. Instead, use mapreduce.task.io.sort.factor
20/03/01 15:16:25 INFO deprecation: mapred.map.child.java.opts is deprecated. Instead, use mapreduce.map.java.opts
20/03/01 15:16:25 INFO deprecation: io.sort.mb is deprecated. Instead, use mapreduce.task.io.sort.mb
20/03/01 15:16:25 INFO deprecation: mapred.task.timeout is deprecated. Instead, use mapreduce.task.timeout
20/03/01 15:16:25 INFO RMProxy: Connecting to ResourceManager at ip-172-31-19-194.ec2.internal/172.31.19.194:8032
20/03/01 15:16:26 INFO FileInputFormat: Total input files to process : 3
20/03/01 15:16:26 INFO JobSubmitter: number of splits:3
20/03/01 15:16:26 INFO JobSubmitter: Submitting tokens for job: job_1583075263102_0010
20/03/01 15:16:26 INFO YarnClientImpl: Submitted application application_1583075263102_0010
20/03/01 15:16:27 INFO Job: The url to track the job: http://ip-172-31-19-194.ec2.internal:20888/proxy/application_1583075263102_0010/
20/03/01 15:16:27 INFO Job: Running job: job_1583075263102_0010
20/03/01 15:16:32 INFO Job: Job job_1583075263102_0010 running in uber mode : false
20/03/01 15:16:32 INFO Job:  map 0% reduce 0%
20/03/01 15:16:39 INFO Job:  map 33% reduce 0%
20/03/01 15:16:41 INFO Job:  map 100% reduce 0%
20/03/01 15:16:54 INFO Job:  map 100% reduce 33%
20/03/01 15:16:57 INFO Job:  map 100% reduce 67%
20/03/01 15:16:58 INFO Job:  map 100% reduce 100%
20/03/01 15:16:58 INFO Job: Job job_1583075263102_0010 completed successfully
20/03/01 15:16:58 INFO Job: Counters: 50
	File System Counters
		FILE: Number of bytes read=147843256
		FILE: Number of bytes written=287539454
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=8437024
		HDFS: Number of bytes written=592397
		HDFS: Number of read operations=42
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=6
	Job Counters 
		Killed reduce tasks=1
		Launched map tasks=3
		Launched reduce tasks=4
		Data-local map tasks=3
		Total time spent by all maps in occupied slots (ms)=1657056
		Total time spent by all reduces in occupied slots (ms)=8650176
		Total time spent by all map tasks (ms)=17261
		Total time spent by all reduce tasks (ms)=45053
		Total vcore-milliseconds taken by all map tasks=17261
		Total vcore-milliseconds taken by all reduce tasks=45053
		Total megabyte-milliseconds taken by all map tasks=53025792
		Total megabyte-milliseconds taken by all reduce tasks=276805632
	Map-Reduce Framework
		Map input records=3678
		Map output records=245896
		Map output bytes=151421513
		Map output materialized bytes=138672589
		Input split bytes=456
		Combine input records=0
		Combine output records=0
		Reduce input groups=6040
		Reduce shuffle bytes=138672589
		Reduce input records=245896
		Reduce output records=6040
		Spilled Records=491792
		Shuffled Maps =9
		Failed Shuffles=0
		Merged Map outputs=9
		GC time elapsed (ms)=1538
		CPU time spent (ms)=64060
		Physical memory (bytes) snapshot=5948874752
		Virtual memory (bytes) snapshot=35781812224
		Total committed heap usage (bytes)=6046613504
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=8301337
	File Output Format Counters 
		Bytes Written=592397
20/03/01 15:16:58 INFO MahoutDriver: Program took 219597 ms (Minutes: 3.65995)
[hadoop@ip-172-31-19-194 ~]$ hadoop fs -ls recommendations
Found 4 items
-rw-r--r--   1 hadoop hadoop          0 2020-03-01 15:16 recommendations/_SUCCESS
-rw-r--r--   1 hadoop hadoop     196721 2020-03-01 15:16 recommendations/part-r-00000
-rw-r--r--   1 hadoop hadoop     197894 2020-03-01 15:16 recommendations/part-r-00001
-rw-r--r--   1 hadoop hadoop     197782 2020-03-01 15:16 recommendations/part-r-00002
[hadoop@ip-172-31-19-194 ~]$ hadoop fs -cat recommendations/part-r-00000 | head
3	[2375:5.0,3494:5.0,2111:5.0,2968:5.0,2243:5.0,3168:5.0,594:5.0,1517:5.0,3035:5.0,2640:5.0]
6	[2572:5.0,2110:5.0,594:5.0,1187:5.0,3035:5.0,2111:5.0,3298:5.0,3826:5.0,3100:5.0,3034:5.0]
9	[265:5.0,196:5.0,3100:5.0,1912:5.0,1320:5.0,1449:5.0,198:5.0,1253:5.0,1517:5.0,1124:5.0]
12	[800:5.0,596:5.0,1252:5.0,930:5.0,3098:5.0,3039:5.0,594:5.0,2641:5.0,2248:5.0,1394:5.0]
15	[3189:5.0,3354:4.525626,3949:4.5074253,3481:4.5030675,3565:4.4976788,3948:4.492203,3751:4.487341,3784:4.4820166,3821:4.479613,3555:4.470099]
18	[2376:5.0,3168:5.0,3035:5.0,2112:5.0,2243:5.0,1320:5.0,3430:5.0,2110:5.0,1187:5.0,1449:5.0]
21	[3409:5.0,1297:5.0,3863:5.0,2692:5.0,3160:5.0,3785:5.0,2600:5.0,3317:5.0,3617:5.0,3328:5.0]
24	[2374:5.0,3168:5.0,3035:5.0,529:5.0,2375:5.0,2243:5.0,2111:5.0,2110:5.0,265:5.0,1449:5.0]
27	[3629:5.0,2375:5.0,2968:5.0,3035:5.0,2967:5.0,2706:5.0,1253:5.0,594:5.0,1188:5.0,2243:5.0]
30	[3098:5.0,1234:5.0,1299:5.0,1645:5.0,555:5.0,1957:5.0,1090:4.7989655,1267:4.752138,2739:4.746989,953:4.736612]
cat: Unable to write to output stream.
[hadoop@ip-172-31-19-194 ~]$ sudo easy_install twisted
Searching for twisted
Reading https://pypi.python.org/simple/twisted/
Downloading https://files.pythonhosted.org/packages/0b/95/5fff90cd4093c79759d736e5f7c921c8eb7e5057a70d753cdb4e8e5895d7/Twisted-19.10.0.tar.bz2#sha256=7394ba7f272ae722a74f3d969dcf599bc4ef093bc392038748a490f1724a515d
Best match: Twisted 19.10.0
Processing Twisted-19.10.0.tar.bz2
Writing /tmp/easy_install-E1GLrq/Twisted-19.10.0/setup.cfg
Running Twisted-19.10.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-E1GLrq/Twisted-19.10.0/egg-dist-tmp-yMePI3
no previously-included directories found matching '.travis'
no previously-included directories found matching 'tests'
warning: no previously-included files found matching 'examplesetup.py'
no previously-included directories found matching 'src/exampleproj'
no previously-included directories found matching 'src/incremental/newsfragments'

Installed /mnt/tmp/easy_install-E1GLrq/Twisted-19.10.0/.eggs/incremental-17.5.0-py2.7.egg
/usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'long_description_content_type'
  warnings.warn(msg)
/usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'project_urls'
  warnings.warn(msg)
warning: no previously-included files matching '*.misc' found under directory 'src/twisted'
warning: no previously-included files matching '*.bugfix' found under directory 'src/twisted'
warning: no previously-included files matching '*.doc' found under directory 'src/twisted'
warning: no previously-included files matching '*.feature' found under directory 'src/twisted'
warning: no previously-included files matching '*.removal' found under directory 'src/twisted'
warning: no previously-included files matching 'NEWS' found under directory 'src/twisted'
warning: no previously-included files matching 'README' found under directory 'src/twisted'
warning: no previously-included files matching 'newsfragments' found under directory 'src/twisted'
warning: no previously-included files found matching 'src/twisted/topfiles/CREDITS'
warning: no previously-included files found matching 'src/twisted/topfiles/ChangeLog.Old'
warning: no previously-included files found matching 'pyproject.toml'
warning: no previously-included files found matching 'codecov.yml'
warning: no previously-included files found matching 'appveyor.yml'
warning: no previously-included files found matching '.coveralls.yml'
warning: no previously-included files found matching '.circleci'
warning: no previously-included files matching '*' found under directory '.circleci'
no previously-included directories found matching 'bin'
no previously-included directories found matching 'admin'
no previously-included directories found matching '.travis'
no previously-included directories found matching '.github'
warning: no previously-included files found matching 'docs/historic/2003'
warning: no previously-included files matching '*' found under directory 'docs/historic/2003'
creating /usr/local/lib/python2.7/site-packages/Twisted-19.10.0-py2.7-linux-x86_64.egg
Extracting Twisted-19.10.0-py2.7-linux-x86_64.egg to /usr/local/lib/python2.7/site-packages
Adding Twisted 19.10.0 to easy-install.pth file
Installing trial script to /usr/local/bin
Installing conch script to /usr/local/bin
Installing ckeygen script to /usr/local/bin
Installing twist script to /usr/local/bin
Installing pyhtmlizer script to /usr/local/bin
Installing mailmail script to /usr/local/bin
Installing tkconch script to /usr/local/bin
Installing twistd script to /usr/local/bin
Installing cftp script to /usr/local/bin

Installed /usr/local/lib/python2.7/site-packages/Twisted-19.10.0-py2.7-linux-x86_64.egg
Processing dependencies for twisted
Searching for attrs>=17.4.0
Reading https://pypi.python.org/simple/attrs/
Downloading https://files.pythonhosted.org/packages/98/c3/2c227e66b5e896e15ccdae2e00bbc69aa46e9a8ce8869cc5fa96310bf612/attrs-19.3.0.tar.gz#sha256=f7b7ce16570fe9965acd6d30101a28f62fb4a7f9e926b3bbc9b61f8b04247e72
Best match: attrs 19.3.0
Processing attrs-19.3.0.tar.gz
Writing /tmp/easy_install-Zkhaoz/attrs-19.3.0/setup.cfg
Running attrs-19.3.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-Zkhaoz/attrs-19.3.0/egg-dist-tmp-oOdeS0
/usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'project_urls'
  warnings.warn(msg)
/usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'long_description_content_type'
  warnings.warn(msg)
no previously-included directories found matching 'docs/_build'
creating /usr/local/lib/python2.7/site-packages/attrs-19.3.0-py2.7.egg
Extracting attrs-19.3.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding attrs 19.3.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/attrs-19.3.0-py2.7.egg
Searching for PyHamcrest>=1.9.0
Reading https://pypi.python.org/simple/PyHamcrest/
Downloading https://files.pythonhosted.org/packages/61/78/807610b7abeb2372e6f5e49b3731dae93547bcb74d02b94de2e265a223d2/PyHamcrest-2.0.1.tar.gz#sha256=9f12b9b3b109d4877b2a1a42a9fb8acbf350213d3b1873eecf6eb07d6ac3caef
Best match: PyHamcrest 2.0.1
Processing PyHamcrest-2.0.1.tar.gz
Writing /tmp/easy_install-EgsyP7/PyHamcrest-2.0.1/setup.cfg
Running PyHamcrest-2.0.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-EgsyP7/PyHamcrest-2.0.1/egg-dist-tmp-818Bmb
warning: no files found matching 'README.md'
  File "build/bdist.linux-x86_64/egg/hamcrest/core/assert_that.py", line 19
    def assert_that(actual: T, matcher: Matcher[T], reason: str = "") -> None:
                          ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/base_description.py", line 18
    def append_text(self, text: str) -> Description:
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/base_matcher.py", line 25
    def __str__(self) -> str:
                      ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/description.py", line 16
    def append_text(self, text: str) -> "Description":
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/matcher.py", line 28
    def matches(self, item: T, mismatch_description: Optional[Description] = None) -> bool:
                          ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/selfdescribing.py", line 11
    def describe_to(self, description: Description) -> None:
                                     ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/selfdescribingvalue.py", line 22
    def __init__(self, value: Any) -> None:
                            ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/string_description.py", line 10
    def tostring(selfdescribing: SelfDescribing) -> str:
                               ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/allof.py", line 16
    def __init__(self, *matchers: Matcher[T], **kwargs):
                                ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/anyof.py", line 16
    def __init__(self, *matchers: Matcher[T]) -> None:
                                ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/described_as.py", line 17
    self, description_template: str, matcher: Matcher[Any], *values: Tuple[Any, ...]
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/is_.py", line 18
    def __init__(self, matcher: Matcher[T]) -> None:
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/isanything.py", line 13
    def __init__(self, description: Optional[str]) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/isequal.py", line 13
    def __init__(self, equals: Any) -> None:
                             ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/isinstanceof.py", line 14
    def __init__(self, expected_type: Type) -> None:
                                    ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/isnone.py", line 15
    def _matches(self, item: Any) -> bool:
                           ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/isnot.py", line 18
    def __init__(self, matcher: Matcher[T]) -> None:
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/issame.py", line 15
    def __init__(self, obj: T) -> None:
                          ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/core/raises.py", line 17
    self, expected: Exception, pattern: Optional[str] = None, matching: Optional[Matcher] = None
                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/helpers/hasmethod.py", line 6
    def hasmethod(obj: object, methodname: str) -> bool:
                     ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/helpers/ismock.py", line 18
    def ismock(obj: Any) -> bool:
                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/core/helpers/wrap_matcher.py", line 13
    def wrap_matcher(x: Union[Matcher[T], T]) -> Matcher[T]:
                      ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/is_empty.py", line 13
    def matches(self, item: Sized, mismatch_description: Optional[Description] = None) -> bool:
                          ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/isdict_containing.py", line 19
    def __init__(self, key_matcher: Matcher[K], value_matcher: Matcher[V]) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/isdict_containingentries.py", line 17
    def __init__(self, value_matchers) -> None:
                                       ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/isdict_containingkey.py", line 17
    def __init__(self, key_matcher: Matcher[K]) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/isdict_containingvalue.py", line 17
    def __init__(self, value_matcher: Matcher[V]) -> None:
                                    ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/isin.py", line 15
    def __init__(self, sequence: Sequence[T]) -> None:
                               ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/issequence_containing.py", line 17
    def __init__(self, element_matcher: Matcher[T]) -> None:
                                      ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/issequence_containinginanyorder.py", line 17
    self, matchers: Sequence[Matcher[T]], mismatch_description: Optional[Description]
                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/issequence_containinginorder.py", line 18
    self, matchers: Sequence[Matcher[T]], mismatch_description: Optional[Description]
                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/collection/issequence_onlycontaining.py", line 17
    def __init__(self, matcher: Matcher[T]) -> None:
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/integration/match_equality.py", line 14
    def __init__(self, matcher: Matcher) -> None:
                              ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/number/iscloseto.py", line 16
    def isnumeric(value: Any) -> bool:
                       ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/number/ordering_comparison.py", line 16
    value: Any,
         ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/object/haslength.py", line 16
    def __init__(self, len_matcher: Matcher[int]) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/object/hasproperty.py", line 20
    def __init__(self, property_name: str, value_matcher: Matcher[V]) -> None:
                                    ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/object/hasstring.py", line 12
    def __init__(self, str_matcher: Matcher[str]) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/isequal_ignoring_case.py", line 11
    def __init__(self, string: str) -> None:
                             ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/isequal_ignoring_whitespace.py", line 10
    def stripspace(string: str) -> str:
                         ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/stringcontains.py", line 11
    def __init__(self, substring) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/stringcontainsinorder.py", line 12
    def __init__(self, *substrings) -> None:
                                    ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/stringendswith.py", line 11
    def __init__(self, substring) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/stringmatches.py", line 14
    def __init__(self, pattern) -> None:
                                ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/stringstartswith.py", line 11
    def __init__(self, substring) -> None:
                                  ^
SyntaxError: invalid syntax

  File "build/bdist.linux-x86_64/egg/hamcrest/library/text/substringmatcher.py", line 11
    class SubstringMatcher(BaseMatcher[str], metaclass=ABCMeta):
                                                      ^
SyntaxError: invalid syntax

zip_safe flag not set; analyzing archive contents...
Copying PyHamcrest-2.0.1-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding PyHamcrest 2.0.1 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/PyHamcrest-2.0.1-py2.7.egg
Searching for hyperlink>=17.1.1
Reading https://pypi.python.org/simple/hyperlink/
Downloading https://files.pythonhosted.org/packages/e0/46/1451027b513a75edf676d25a47f601ca00b06a6a7a109e5644d921e7462d/hyperlink-19.0.0.tar.gz#sha256=4288e34705da077fada1111a24a0aa08bb1e76699c9ce49876af722441845654
Best match: hyperlink 19.0.0
Processing hyperlink-19.0.0.tar.gz
Writing /tmp/easy_install-cEKCdK/hyperlink-19.0.0/setup.cfg
Running hyperlink-19.0.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-cEKCdK/hyperlink-19.0.0/egg-dist-tmp-_TnGRn
warning: no files found matching '.coveragerc'
warning: no files found matching 'Makefile'
warning: no previously-included files found matching 'TODO.md'
warning: no previously-included files found matching 'appveyor.yml'
no previously-included directories found matching 'docs/_build'
creating /usr/local/lib/python2.7/site-packages/hyperlink-19.0.0-py2.7.egg
Extracting hyperlink-19.0.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding hyperlink 19.0.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/hyperlink-19.0.0-py2.7.egg
Searching for Automat>=0.3.0
Reading https://pypi.python.org/simple/Automat/
Downloading https://files.pythonhosted.org/packages/80/c5/82c63bad570f4ef745cc5c2f0713c8eddcd07153b4bee7f72a8dc9f9384b/Automat-20.2.0.tar.gz#sha256=7979803c74610e11ef0c0d68a2942b152df52da55336e0c9d58daf1831cbdf33
Best match: Automat 20.2.0
Processing Automat-20.2.0.tar.gz
Writing /tmp/easy_install-MW8hIZ/Automat-20.2.0/setup.cfg
Running Automat-20.2.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-MW8hIZ/Automat-20.2.0/egg-dist-tmp-Sk7tqH


!!! m2r not found, long_description is bad, don't upload this to PyPI !!!


warning: no previously-included files matching '__pycache__' found under directory '*'
warning: no previously-included files matching '*.py[co]' found under directory '*'

Installed /mnt/tmp/easy_install-MW8hIZ/Automat-20.2.0/.eggs/m2r-0.2.1-py2.7.egg
Searching for setuptools-scm
Reading https://pypi.python.org/simple/setuptools-scm/
Downloading https://files.pythonhosted.org/packages/0a/6c/06de5bb5c6a745a651b97d3bcc8cd1cc0f926688ff5c0489e3ff8400c22f/setuptools_scm-3.5.0-py2.7.egg#sha256=fa6511072840d7eaad3ea36cc8849f437fefd85b32499a7f6026cba16ebbf63a
Best match: setuptools-scm 3.5.0
Processing setuptools_scm-3.5.0-py2.7.egg
Moving setuptools_scm-3.5.0-py2.7.egg to /mnt/tmp/easy_install-MW8hIZ/Automat-20.2.0/.eggs

Installed /mnt/tmp/easy_install-MW8hIZ/Automat-20.2.0/.eggs/setuptools_scm-3.5.0-py2.7.egg
Searching for mistune
Reading https://pypi.python.org/simple/mistune/
Downloading https://files.pythonhosted.org/packages/76/0d/2f9837daa53af53f665a43ad10c167a721160c872f836cc736a39dd1a284/mistune-2.0.0a2.tar.gz#sha256=dffb6b7a92a341b95e5432fab8532f2731f903165c25fe2fbef8f4218374acaa
Best match: mistune 2.0.0a2
Processing mistune-2.0.0a2.tar.gz
Writing /tmp/easy_install-MW8hIZ/Automat-20.2.0/temp/easy_install-wnkv3N/mistune-2.0.0a2/setup.cfg
Running mistune-2.0.0a2/setup.py -q bdist_egg --dist-dir /tmp/easy_install-MW8hIZ/Automat-20.2.0/temp/easy_install-wnkv3N/mistune-2.0.0a2/egg-dist-tmp-EeH3ZR
warning: no files found matching 'CHANGES.rst'
warning: no previously-included files found matching 'mistune.c'
warning: no previously-included files matching '*.pyc' found under directory 'tests'
warning: no previously-included files matching '*.pyo' found under directory 'tests'
creating /mnt/tmp/easy_install-MW8hIZ/Automat-20.2.0/.eggs/mistune-2.0.0a2-py2.7.egg
Extracting mistune-2.0.0a2-py2.7.egg to /mnt/tmp/easy_install-MW8hIZ/Automat-20.2.0/.eggs

Installed /mnt/tmp/easy_install-MW8hIZ/Automat-20.2.0/.eggs/mistune-2.0.0a2-py2.7.egg
zip_safe flag not set; analyzing archive contents...
Copying Automat-20.2.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding Automat 20.2.0 to easy-install.pth file
Installing automat-visualize script to /usr/local/bin

Installed /usr/local/lib/python2.7/site-packages/Automat-20.2.0-py2.7.egg
Searching for incremental>=16.10.1
Reading https://pypi.python.org/simple/incremental/
Downloading https://files.pythonhosted.org/packages/8f/26/02c4016aa95f45479eea37c90c34f8fab6775732ae62587a874b619ca097/incremental-17.5.0.tar.gz#sha256=7b751696aaf36eebfab537e458929e194460051ccad279c72b755a167eebd4b3
Best match: incremental 17.5.0
Processing incremental-17.5.0.tar.gz
Writing /tmp/easy_install-HCsdSr/incremental-17.5.0/setup.cfg
Running incremental-17.5.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-HCsdSr/incremental-17.5.0/egg-dist-tmp-VFcjmK
no previously-included directories found matching '.travis'
no previously-included directories found matching 'tests'
warning: no previously-included files found matching 'examplesetup.py'
no previously-included directories found matching 'src/exampleproj'
no previously-included directories found matching 'src/incremental/newsfragments'
creating /usr/local/lib/python2.7/site-packages/incremental-17.5.0-py2.7.egg
Extracting incremental-17.5.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding incremental 17.5.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/incremental-17.5.0-py2.7.egg
Searching for constantly>=15.1
Reading https://pypi.python.org/simple/constantly/
Downloading https://files.pythonhosted.org/packages/95/f1/207a0a478c4bb34b1b49d5915e2db574cadc415c9ac3a7ef17e29b2e8951/constantly-15.1.0.tar.gz#sha256=586372eb92059873e29eba4f9dec8381541b4d3834660707faf8ba59146dfc35
Best match: constantly 15.1.0
Processing constantly-15.1.0.tar.gz
Writing /tmp/easy_install-GIed8u/constantly-15.1.0/setup.cfg
Running constantly-15.1.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-GIed8u/constantly-15.1.0/egg-dist-tmp-xCj5IV
UPDATING build/lib/constantly/_version.py
set build/lib/constantly/_version.py to '15.1.0'
zip_safe flag not set; analyzing archive contents...
Copying constantly-15.1.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding constantly 15.1.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/constantly-15.1.0-py2.7.egg
Searching for zope.interface>=4.4.2
Reading https://pypi.python.org/simple/zope.interface/
Downloading https://files.pythonhosted.org/packages/c3/05/bf3130eb799548882ce61b7c3d2dbc5d4d5cc6e821efa8786c5273d56844/zope.interface-4.7.1.tar.gz#sha256=4bb937e998be9d5e345f486693e477ba79e4344674484001a0b646be1d530487
Best match: zope.interface 4.7.1
Processing zope.interface-4.7.1.tar.gz
Writing /tmp/easy_install-TGQeoG/zope.interface-4.7.1/setup.cfg
Running zope.interface-4.7.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-TGQeoG/zope.interface-4.7.1/egg-dist-tmp-kozdwH
warning: no previously-included files matching '*.dll' found anywhere in distribution
warning: no previously-included files matching '*.pyc' found anywhere in distribution
warning: no previously-included files matching '*.pyo' found anywhere in distribution
warning: no previously-included files matching '*.so' found anywhere in distribution
warning: no previously-included files matching 'coverage.xml' found anywhere in distribution
warning: no previously-included files matching 'appveyor.yml' found anywhere in distribution
no previously-included directories found matching 'docs/_build'
creating /usr/local/lib/python2.7/site-packages/zope.interface-4.7.1-py2.7-linux-x86_64.egg
Extracting zope.interface-4.7.1-py2.7-linux-x86_64.egg to /usr/local/lib/python2.7/site-packages
Adding zope.interface 4.7.1 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/zope.interface-4.7.1-py2.7-linux-x86_64.egg
Searching for idna>=2.5
Reading https://pypi.python.org/simple/idna/
Downloading https://files.pythonhosted.org/packages/cb/19/57503b5de719ee45e83472f339f617b0c01ad75cba44aba1e4c97c2b0abd/idna-2.9.tar.gz#sha256=7588d1c14ae4c77d74036e8c22ff447b26d0fde8f007354fd48a7814db15b7cb
Best match: idna 2.9
Processing idna-2.9.tar.gz
Writing /tmp/easy_install-6axvGL/idna-2.9/setup.cfg
Running idna-2.9/setup.py -q bdist_egg --dist-dir /tmp/easy_install-6axvGL/idna-2.9/egg-dist-tmp-LDb8n8
warning: no previously-included files matching '*.pyc' found under directory 'tools'
warning: no previously-included files matching '*.pyc' found under directory 'tests'
zip_safe flag not set; analyzing archive contents...
Copying idna-2.9-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding idna 2.9 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/idna-2.9-py2.7.egg
Finished processing dependencies for twisted
[hadoop@ip-172-31-19-194 ~]$ sudo easy_install klein
Searching for klein
Reading https://pypi.python.org/simple/klein/
Downloading https://files.pythonhosted.org/packages/3f/2d/37fd56faaae3a810728ef1ce2494bac491795df8aabe413e39de5fdcb201/klein-19.6.0.tar.gz#sha256=e7b76e5f8fbac5bce598ce96ac73a19f4117afb8eba9cde2ff05e772d433cd93
Best match: klein 19.6.0
Processing klein-19.6.0.tar.gz
Writing /tmp/easy_install-Q64Kdy/klein-19.6.0/setup.cfg
Running klein-19.6.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-Q64Kdy/klein-19.6.0/egg-dist-tmp-xf_t99
  File "build/bdist.linux-x86_64/egg/klein/test/py3_test_resource.py", line 25
    async def leaf(request):
        ^
SyntaxError: invalid syntax

zip_safe flag not set; analyzing archive contents...
klein.test.test_resource: module references __file__
klein.test._strategies: module references __file__
creating /usr/local/lib/python2.7/site-packages/klein-19.6.0-py2.7.egg
Extracting klein-19.6.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
  File "/usr/local/lib/python2.7/site-packages/klein-19.6.0-py2.7.egg/klein/test/py3_test_resource.py", line 25
    async def leaf(request):
        ^
SyntaxError: invalid syntax

Adding klein 19.6.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/klein-19.6.0-py2.7.egg
Processing dependencies for klein
Searching for enum34
Reading https://pypi.python.org/simple/enum34/
Downloading https://files.pythonhosted.org/packages/a4/04/94310230b388f6c1992cec62716229b5f2258ad14002346509152d398e3e/enum34-1.1.9.tar.gz#sha256=13ef9a1c478203252107f66c25b99b45b1865693ca1284aab40dafa7e1e7ac17
Best match: enum34 1.1.9
Processing enum34-1.1.9.tar.gz
Writing /tmp/easy_install-Ffki2E/enum34-1.1.9/setup.cfg
Running enum34-1.1.9/setup.py -q bdist_egg --dist-dir /tmp/easy_install-Ffki2E/enum34-1.1.9/egg-dist-tmp-P9bi1_
warning: no files found matching 'enum/doc/enum.pdf'
zip_safe flag not set; analyzing archive contents...
Copying enum34-1.1.9-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding enum34 1.1.9 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/enum34-1.1.9-py2.7.egg
Searching for Werkzeug
Reading https://pypi.python.org/simple/Werkzeug/
Downloading https://files.pythonhosted.org/packages/4b/a5/781dbff5062f31e8407242ea2e07c05eb4f3a236f59124ef46f5e92a2776/Werkzeug-1.0.0.tar.gz#sha256=169ba8a33788476292d04186ab33b01d6add475033dfc07215e6d219cc077096
Best match: Werkzeug 1.0.0
Processing Werkzeug-1.0.0.tar.gz
Writing /tmp/easy_install-5mHs52/Werkzeug-1.0.0/setup.cfg
Running Werkzeug-1.0.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-5mHs52/Werkzeug-1.0.0/egg-dist-tmp-wkj7rA
/usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'project_urls'
  warnings.warn(msg)
no previously-included directories found matching 'docs/_build'
warning: no previously-included files matching '*.py[co]' found anywhere in distribution
zip_safe flag not set; analyzing archive contents...
werkzeug._reloader: module references __file__
werkzeug.utils: module references __file__
werkzeug.utils: module references __path__
werkzeug.debug.__init__: module references __file__
werkzeug.debug.tbtools: module MAY be using inspect.getsourcefile
creating /usr/local/lib/python2.7/site-packages/Werkzeug-1.0.0-py2.7.egg
Extracting Werkzeug-1.0.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding Werkzeug 1.0.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/Werkzeug-1.0.0-py2.7.egg
Searching for typing
Reading https://pypi.python.org/simple/typing/
Downloading https://files.pythonhosted.org/packages/67/b0/b2ea2bd67bfb80ea5d12a5baa1d12bda002cab3b6c9b48f7708cd40c34bf/typing-3.7.4.1.tar.gz#sha256=91dfe6f3f706ee8cc32d38edbbf304e9b7583fb37108fef38229617f8b3eba23
Best match: typing 3.7.4.1
Processing typing-3.7.4.1.tar.gz
Writing /tmp/easy_install-D53Zka/typing-3.7.4.1/setup.cfg
Running typing-3.7.4.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-D53Zka/typing-3.7.4.1/egg-dist-tmp-C71wPJ
zip_safe flag not set; analyzing archive contents...
Copying typing-3.7.4.1-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding typing 3.7.4.1 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/typing-3.7.4.1-py2.7.egg
Searching for Tubes
Reading https://pypi.python.org/simple/Tubes/
Downloading https://files.pythonhosted.org/packages/51/7f/cee2e4c6315cbb2dccb2e2c65e41e032c973dcd5d5799a2dc99b410ac992/Tubes-0.1.0.tar.gz#sha256=dab2323784925d4e19ac4514a5be0f89f91e107cc0efc8e4e988c5f4f69ddd49
Best match: Tubes 0.1.0
Processing Tubes-0.1.0.tar.gz
Writing /tmp/easy_install-6l4TvK/Tubes-0.1.0/setup.cfg
Running Tubes-0.1.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-6l4TvK/Tubes-0.1.0/egg-dist-tmp-uSVTiL
zip_safe flag not set; analyzing archive contents...
Copying Tubes-0.1.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding Tubes 0.1.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/Tubes-0.1.0-py2.7.egg
Searching for characteristic
Reading https://pypi.python.org/simple/characteristic/
Downloading https://files.pythonhosted.org/packages/dc/66/54b7a4758ea44fbc93895c7745060005272560fb2c356f2a6f7448ef9a80/characteristic-14.3.0.tar.gz#sha256=ded68d4e424115ed44e5c83c2a901a0b6157a959079d7591d92106ffd3ada380
Best match: characteristic 14.3.0
Processing characteristic-14.3.0.tar.gz
Writing /tmp/easy_install-zv4Vxh/characteristic-14.3.0/setup.cfg
Running characteristic-14.3.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-zv4Vxh/characteristic-14.3.0/egg-dist-tmp-EWTQ6w
no previously-included directories found matching 'benchmark.py'
no previously-included directories found matching 'docs/_build'
zip_safe flag not set; analyzing archive contents...
Copying characteristic-14.3.0-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding characteristic 14.3.0 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/characteristic-14.3.0-py2.7.egg
Finished processing dependencies for klein
[hadoop@ip-172-31-19-194 ~]$ sudo easy_install redis
Searching for redis
Reading https://pypi.python.org/simple/redis/
Downloading https://files.pythonhosted.org/packages/ef/2e/2c0f59891db7db087a7eeaa79bc7c7f2c039e71a2b5b0a41391e9d462926/redis-3.4.1.tar.gz#sha256=0dcfb335921b88a850d461dc255ff4708294943322bd55de6cfd68972490ca1f
Best match: redis 3.4.1
Processing redis-3.4.1.tar.gz
Writing /tmp/easy_install-WRefca/redis-3.4.1/setup.cfg
Running redis-3.4.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-WRefca/redis-3.4.1/egg-dist-tmp-haJ8xK
/usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'long_description_content_type'
  warnings.warn(msg)
warning: no previously-included files found matching '__pycache__'
warning: no previously-included files matching '*.pyc' found under directory 'tests'
zip_safe flag not set; analyzing archive contents...
Copying redis-3.4.1-py2.7.egg to /usr/local/lib/python2.7/site-packages
Adding redis 3.4.1 to easy-install.pth file

Installed /usr/local/lib/python2.7/site-packages/redis-3.4.1-py2.7.egg
Processing dependencies for redis
Finished processing dependencies for redis
[hadoop@ip-172-31-19-194 ~]$ wget http://download.redis.io/releases/redis-2.8.7.tar.gz
--2020-03-01 15:18:27--  http://download.redis.io/releases/redis-2.8.7.tar.gz
Resolving download.redis.io (download.redis.io)... 109.74.203.151
Connecting to download.redis.io (download.redis.io)|109.74.203.151|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1064262 (1.0M) [application/x-gzip]
Saving to: ‘redis-2.8.7.tar.gz’

redis-2.8.7.tar.gz                             100%[===================================================================================================>]   1.01M  2.05MB/s    in 0.5s    

2020-03-01 15:18:27 (2.05 MB/s) - ‘redis-2.8.7.tar.gz’ saved [1064262/1064262]

[hadoop@ip-172-31-19-194 ~]$ tar xzf redis-2.8.7.tar.gz
[hadoop@ip-172-31-19-194 ~]$ cd redis-2.8.7
[hadoop@ip-172-31-19-194 redis-2.8.7]$ make
cd src && make all
make[1]: Entering directory `/home/hadoop/redis-2.8.7/src'
rm -rf redis-server redis-sentinel redis-cli redis-benchmark redis-check-dump redis-check-aof *.o *.gcda *.gcno *.gcov redis.info lcov-html
(cd ../deps && make distclean)
make[2]: Entering directory `/home/hadoop/redis-2.8.7/deps'
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
make[2]: Leaving directory `/home/hadoop/redis-2.8.7/deps'
(rm -f .make-*)
echo STD=-std=c99 -pedantic >> .make-settings
echo WARN=-Wall >> .make-settings
echo OPT=-O2 >> .make-settings
echo MALLOC=jemalloc >> .make-settings
echo CFLAGS= >> .make-settings
echo LDFLAGS= >> .make-settings
echo REDIS_CFLAGS= >> .make-settings
echo REDIS_LDFLAGS= >> .make-settings
echo PREV_FINAL_CFLAGS=-std=c99 -pedantic -Wall -O2 -g -ggdb   -I../deps/hiredis -I../deps/linenoise -I../deps/lua/src -DUSE_JEMALLOC -I../deps/jemalloc/include >> .make-settings
echo PREV_FINAL_LDFLAGS=  -g -ggdb -rdynamic >> .make-settings
(cd ../deps && make hiredis linenoise lua jemalloc)
make[2]: Entering directory `/home/hadoop/redis-2.8.7/deps'
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
(echo "" > .make-cflags)
(echo "" > .make-ldflags)
MAKE hiredis
cd hiredis && make static
make[3]: Entering directory `/home/hadoop/redis-2.8.7/deps/hiredis'
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  net.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  hiredis.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  sds.c
cc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  async.c
ar rcs libhiredis.a net.o hiredis.o sds.o async.o
make[3]: Leaving directory `/home/hadoop/redis-2.8.7/deps/hiredis'
MAKE linenoise
cd linenoise && make
make[3]: Entering directory `/home/hadoop/redis-2.8.7/deps/linenoise'
cc  -Wall -Os -g  -c linenoise.c
make[3]: Leaving directory `/home/hadoop/redis-2.8.7/deps/linenoise'
MAKE lua
cd lua/src && make all CFLAGS="-O2 -Wall -DLUA_ANSI " MYLDFLAGS=""
make[3]: Entering directory `/home/hadoop/redis-2.8.7/deps/lua/src'
cc -O2 -Wall -DLUA_ANSI    -c -o lapi.o lapi.c
cc -O2 -Wall -DLUA_ANSI    -c -o lcode.o lcode.c
cc -O2 -Wall -DLUA_ANSI    -c -o ldebug.o ldebug.c
cc -O2 -Wall -DLUA_ANSI    -c -o ldo.o ldo.c
cc -O2 -Wall -DLUA_ANSI    -c -o ldump.o ldump.c
cc -O2 -Wall -DLUA_ANSI    -c -o lfunc.o lfunc.c
cc -O2 -Wall -DLUA_ANSI    -c -o lgc.o lgc.c
cc -O2 -Wall -DLUA_ANSI    -c -o llex.o llex.c
cc -O2 -Wall -DLUA_ANSI    -c -o lmem.o lmem.c
cc -O2 -Wall -DLUA_ANSI    -c -o lobject.o lobject.c
cc -O2 -Wall -DLUA_ANSI    -c -o lopcodes.o lopcodes.c
cc -O2 -Wall -DLUA_ANSI    -c -o lparser.o lparser.c
cc -O2 -Wall -DLUA_ANSI    -c -o lstate.o lstate.c
cc -O2 -Wall -DLUA_ANSI    -c -o lstring.o lstring.c
cc -O2 -Wall -DLUA_ANSI    -c -o ltable.o ltable.c
cc -O2 -Wall -DLUA_ANSI    -c -o ltm.o ltm.c
cc -O2 -Wall -DLUA_ANSI    -c -o lundump.o lundump.c
cc -O2 -Wall -DLUA_ANSI    -c -o lvm.o lvm.c
cc -O2 -Wall -DLUA_ANSI    -c -o lzio.o lzio.c
cc -O2 -Wall -DLUA_ANSI    -c -o strbuf.o strbuf.c
cc -O2 -Wall -DLUA_ANSI    -c -o lauxlib.o lauxlib.c
lauxlib.c: In function ‘luaL_loadfile’:
lauxlib.c:577:4: warning: this ‘while’ clause does not guard... [-Wmisleading-indentation]
    while ((c = getc(lf.f)) != EOF && c != LUA_SIGNATURE[0]) ;
    ^~~~~
lauxlib.c:578:5: note: ...this statement, but the latter is misleadingly indented as if it were guarded by the ‘while’
     lf.extraline = 0;
     ^~
cc -O2 -Wall -DLUA_ANSI    -c -o lbaselib.o lbaselib.c
cc -O2 -Wall -DLUA_ANSI    -c -o ldblib.o ldblib.c
cc -O2 -Wall -DLUA_ANSI    -c -o liolib.o liolib.c
cc -O2 -Wall -DLUA_ANSI    -c -o lmathlib.o lmathlib.c
cc -O2 -Wall -DLUA_ANSI    -c -o loslib.o loslib.c
cc -O2 -Wall -DLUA_ANSI    -c -o ltablib.o ltablib.c
ltablib.c: In function ‘addfield’:
ltablib.c:137:3: warning: this ‘if’ clause does not guard... [-Wmisleading-indentation]
   if (!lua_isstring(L, -1))
   ^~
ltablib.c:140:5: note: ...this statement, but the latter is misleadingly indented as if it were guarded by the ‘if’
     luaL_addvalue(b);
     ^~~~~~~~~~~~~
cc -O2 -Wall -DLUA_ANSI    -c -o lstrlib.o lstrlib.c
cc -O2 -Wall -DLUA_ANSI    -c -o loadlib.o loadlib.c
cc -O2 -Wall -DLUA_ANSI    -c -o linit.o linit.c
cc -O2 -Wall -DLUA_ANSI    -c -o lua_cjson.o lua_cjson.c
cc -O2 -Wall -DLUA_ANSI    -c -o lua_struct.o lua_struct.c
cc -O2 -Wall -DLUA_ANSI    -c -o lua_cmsgpack.o lua_cmsgpack.c
lua_cmsgpack.c: In function ‘table_is_an_array’:
lua_cmsgpack.c:370:21: warning: variable ‘max’ set but not used [-Wunused-but-set-variable]
     long count = 0, max = 0, idx = 0;
                     ^~~
ar rcu liblua.a lapi.o lcode.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundump.o lvm.o lzio.o strbuf.o lauxlib.o lbaselib.o ldblib.o liolib.o lmathlib.o loslib.o ltablib.o lstrlib.o loadlib.o linit.o lua_cjson.o lua_struct.o lua_cmsgpack.o	# DLL needs all object files
ranlib liblua.a
cc -O2 -Wall -DLUA_ANSI    -c -o lua.o lua.c
cc -o lua  lua.o liblua.a -lm 
liblua.a(loslib.o): In function `os_tmpname':
loslib.c:(.text+0x27c): warning: the use of `tmpnam' is dangerous, better use `mkstemp'
cc -O2 -Wall -DLUA_ANSI    -c -o luac.o luac.c
cc -O2 -Wall -DLUA_ANSI    -c -o print.o print.c
cc -o luac  luac.o print.o liblua.a -lm 
make[3]: Leaving directory `/home/hadoop/redis-2.8.7/deps/lua/src'
MAKE jemalloc
cd jemalloc && ./configure --with-jemalloc-prefix=je_ --enable-cc-silence CFLAGS="-std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops " LDFLAGS=""
checking for xsltproc... /usr/bin/xsltproc
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking size of void *... 8
checking size of int... 4
checking size of long... 8
checking size of intmax_t... 8
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking whether __asm__ syntax is compilable... yes
checking whether __attribute__ syntax is compilable... yes
checking whether compiler supports -fvisibility=hidden... yes
checking whether compiler supports -Werror... yes
checking whether tls_model attribute is compilable... no
checking for a BSD-compatible install... /usr/bin/install -c
checking for ranlib... ranlib
checking for ar... /usr/bin/ar
checking for ld... /usr/bin/ld
checking for autoconf... no
checking for memalign... yes
checking for valloc... yes
checking configured backtracing method... N/A
checking for sbrk... yes
checking whether utrace(2) is compilable... no
checking whether valgrind is compilable... no
checking STATIC_PAGE_SHIFT... 12
checking pthread.h usability... yes
checking pthread.h presence... yes
checking for pthread.h... yes
checking for pthread_create in -lpthread... yes
checking for _malloc_thread_cleanup... no
checking for _pthread_mutex_init_calloc_cb... no
checking for TLS... yes
checking whether a program using ffsl is compilable... yes
checking whether atomic(9) is compilable... no
checking whether Darwin OSAtomic*() is compilable... no
checking whether to force 32-bit __sync_{add,sub}_and_fetch()... no
checking whether to force 64-bit __sync_{add,sub}_and_fetch()... no
checking whether Darwin OSSpin*() is compilable... no
checking for stdbool.h that conforms to C99... yes
checking for _Bool... yes
configure: creating ./config.status
config.status: creating Makefile
config.status: creating doc/html.xsl
config.status: creating doc/manpages.xsl
config.status: creating doc/jemalloc.xml
config.status: creating include/jemalloc/jemalloc.h
config.status: creating include/jemalloc/internal/jemalloc_internal.h
config.status: creating test/jemalloc_test.h
config.status: creating config.stamp
config.status: creating bin/jemalloc.sh
config.status: creating include/jemalloc/jemalloc_defs.h
config.status: executing include/jemalloc/internal/size_classes.h commands
===============================================================================
jemalloc version   : 3.2.0-0-g87499f6748ebe4817571e817e9f680ccb5bf54a9
library revision   : 1

CC                 : gcc
CPPFLAGS           :  -D_GNU_SOURCE -D_REENTRANT
CFLAGS             : -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -fvisibility=hidden
LDFLAGS            : 
LIBS               :  -lm -lpthread
RPATH_EXTRA        : 

XSLTPROC           : /usr/bin/xsltproc
XSLROOT            : 

PREFIX             : /usr/local
BINDIR             : /usr/local/bin
INCLUDEDIR         : /usr/local/include
LIBDIR             : /usr/local/lib
DATADIR            : /usr/local/share
MANDIR             : /usr/local/share/man

srcroot            : 
abs_srcroot        : /home/hadoop/redis-2.8.7/deps/jemalloc/
objroot            : 
abs_objroot        : /home/hadoop/redis-2.8.7/deps/jemalloc/

JEMALLOC_PREFIX    : je_
JEMALLOC_PRIVATE_NAMESPACE
                   : 
install_suffix     : 
autogen            : 0
experimental       : 1
cc-silence         : 1
debug              : 0
stats              : 1
prof               : 0
prof-libunwind     : 0
prof-libgcc        : 0
prof-gcc           : 0
tcache             : 1
fill               : 1
utrace             : 0
valgrind           : 0
xmalloc            : 0
mremap             : 0
munmap             : 0
dss                : 0
lazy_lock          : 0
tls                : 1
===============================================================================
cd jemalloc && make CFLAGS="-std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops " LDFLAGS="" lib/libjemalloc.a
make[3]: Entering directory `/home/hadoop/redis-2.8.7/deps/jemalloc'
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/jemalloc.o src/jemalloc.c
src/jemalloc.c: In function ‘je_realloc’:
src/jemalloc.c:1082:9: warning: variable ‘old_rzsize’ set but not used [-Wunused-but-set-variable]
  size_t old_rzsize JEMALLOC_CC_SILENCE_INIT(0);
         ^~~~~~~~~~
src/jemalloc.c: In function ‘je_free’:
src/jemalloc.c:1230:10: warning: variable ‘rzsize’ set but not used [-Wunused-but-set-variable]
   size_t rzsize JEMALLOC_CC_SILENCE_INIT(0);
          ^~~~~~
src/jemalloc.c: In function ‘je_rallocm’:
src/jemalloc.c:1477:9: warning: variable ‘old_rzsize’ set but not used [-Wunused-but-set-variable]
  size_t old_rzsize JEMALLOC_CC_SILENCE_INIT(0);
         ^~~~~~~~~~
src/jemalloc.c: In function ‘je_dallocm’:
src/jemalloc.c:1622:9: warning: variable ‘rzsize’ set but not used [-Wunused-but-set-variable]
  size_t rzsize JEMALLOC_CC_SILENCE_INIT(0);
         ^~~~~~
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/arena.o src/arena.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/atomic.o src/atomic.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/base.o src/base.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/bitmap.o src/bitmap.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/chunk.o src/chunk.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/chunk_dss.o src/chunk_dss.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/chunk_mmap.o src/chunk_mmap.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/ckh.o src/ckh.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/ctl.o src/ctl.c
src/ctl.c: In function ‘epoch_ctl’:
src/ctl.c:1112:11: warning: variable ‘newval’ set but not used [-Wunused-but-set-variable]
  uint64_t newval;
           ^~~~~~
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/extent.o src/extent.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/hash.o src/hash.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/huge.o src/huge.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/mb.o src/mb.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/mutex.o src/mutex.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/prof.o src/prof.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/quarantine.o src/quarantine.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/rtree.o src/rtree.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/stats.o src/stats.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/tcache.o src/tcache.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/util.o src/util.c
gcc -std=gnu99 -Wall -pipe -g3 -O3 -funroll-loops  -c -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -o src/tsd.o src/tsd.c
ar crus lib/libjemalloc.a src/jemalloc.o src/arena.o src/atomic.o src/base.o src/bitmap.o src/chunk.o src/chunk_dss.o src/chunk_mmap.o src/ckh.o src/ctl.o src/extent.o src/hash.o src/huge.o src/mb.o src/mutex.o src/prof.o src/quarantine.o src/rtree.o src/stats.o src/tcache.o src/util.o src/tsd.o
make[3]: Leaving directory `/home/hadoop/redis-2.8.7/deps/jemalloc'
make[2]: Leaving directory `/home/hadoop/redis-2.8.7/deps'
    CC adlist.o
    CC ae.o
    CC anet.o
    CC dict.o
    CC redis.o
    CC sds.o
    CC zmalloc.o
    CC lzf_c.o
In file included from lzf_c.c:37:0:
lzfP.h:131:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
 #if !STRICT_ALIGN
      ^~~~~~~~~~~~
lzfP.h:131:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
lzf_c.c: In function ‘lzf_compress’:
lzf_c.c:158:5: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
 #if STRICT_ALIGN
     ^~~~~~~~~~~~
lzf_c.c:158:5: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
    CC lzf_d.o
In file included from lzf_d.c:37:0:
lzfP.h:131:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
 #if !STRICT_ALIGN
      ^~~~~~~~~~~~
lzfP.h:131:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
    CC pqsort.o
    CC zipmap.o
    CC sha1.o
    CC ziplist.o
    CC release.o
    CC networking.o
    CC util.o
    CC object.o
    CC db.o
    CC replication.o
    CC rdb.o
    CC t_string.o
    CC t_list.o
    CC t_set.o
    CC t_zset.o
    CC t_hash.o
    CC config.o
    CC aof.o
    CC pubsub.o
    CC multi.o
    CC debug.o
    CC sort.o
    CC intset.o
    CC syncio.o
    CC migrate.o
    CC endianconv.o
    CC slowlog.o
    CC scripting.o
    CC bio.o
    CC rio.o
    CC rand.o
    CC memtest.o
    CC crc64.o
    CC bitops.o
    CC sentinel.o
    CC notify.o
    CC setproctitle.o
setproctitle.c:46:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
 #if !HAVE_SETPROCTITLE
      ^~~~~~~~~~~~~~~~~
setproctitle.c:46:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
setproctitle.c:46:6: warning: this use of "defined" may not be portable [-Wexpansion-to-defined]
    LINK redis-server
    INSTALL redis-sentinel
    CC redis-cli.o
    LINK redis-cli
    CC redis-benchmark.o
    LINK redis-benchmark
    CC redis-check-dump.o
    LINK redis-check-dump
    CC redis-check-aof.o
    LINK redis-check-aof

Hint: To run 'make test' is a good idea ;)

make[1]: Leaving directory `/home/hadoop/redis-2.8.7/src'
[hadoop@ip-172-31-19-194 redis-2.8.7]$ ./src/redis-server &
[1] 22895
[hadoop@ip-172-31-19-194 redis-2.8.7]$ [22895] 01 Mar 15:19:29.054 # Warning: no config file specified, using the default config. In order to specify a config file use ./src/redis-server /path/to/redis.conf
[22895] 01 Mar 15:19:29.055 # Unable to set the max number of files limit to 10032 (Operation not permitted), setting the max clients configuration to 3984.
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 2.8.7 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in stand alone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 22895
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

[22895] 01 Mar 15:19:29.055 # Server started, Redis version 2.8.7
[22895] 01 Mar 15:19:29.055 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
[22895] 01 Mar 15:19:29.055 * The server is now ready to accept connections on port 6379

[hadoop@ip-172-31-19-194 redis-2.8.7]$ hadoop fs -ls
Found 3 items
drwxr-xr-x   - hadoop hadoop          0 2020-03-01 15:16 recommendations
drwxr-xr-x   - hadoop hadoop          0 2020-03-01 15:16 similarity-matrix
drwxr-xr-x   - hadoop hadoop          0 2020-03-01 15:16 temp
[hadoop@ip-172-31-19-194 redis-2.8.7]$ hadoop fs -help touchz
-touchz <path> ... :
  Creates a file of zero length at <path> with current time as the timestamp of
  that <path>. An error is returned if the file exists with non-zero length
[hadoop@ip-172-31-19-194 redis-2.8.7]$ hadoop fs -touchz hello.py
[hadoop@ip-172-31-19-194 redis-2.8.7]$ hadoop fs -ls
Found 4 items
-rw-r--r--   1 hadoop hadoop          0 2020-03-01 15:21 hello.py
drwxr-xr-x   - hadoop hadoop          0 2020-03-01 15:16 recommendations
drwxr-xr-x   - hadoop hadoop          0 2020-03-01 15:16 similarity-matrix
drwxr-xr-x   - hadoop hadoop          0 2020-03-01 15:16 temp
[hadoop@ip-172-31-19-194 redis-2.8.7]$ nano hello.py
[hadoop@ip-172-31-19-194 redis-2.8.7]$ nano hello.py
[hadoop@ip-172-31-19-194 redis-2.8.7]$ twistd -noy hello.py &
[2] 24533
[hadoop@ip-172-31-19-194 redis-2.8.7]$ 2020-03-01 15:23:59+0000 [-] Log opened.
2020-03-01 15:23:59+0000 [-] Site starting on 8081
2020-03-01 15:23:59+0000 [-] Starting factory <twisted.web.server.Site instance at 0x7f3d94aaa680>
[22895] 01 Mar 15:24:30.017 * 100 changes in 300 seconds. Saving...
[22895] 01 Mar 15:24:30.017 * Background saving started by pid 24699
[24699] 01 Mar 15:24:30.029 * DB saved on disk
[24699] 01 Mar 15:24:30.029 * RDB: 0 MB of memory used by copy-on-write
[22895] 01 Mar 15:24:30.117 * Background saving terminated with success

[hadoop@ip-172-31-19-194 redis-2.8.7]$ curl localhost:8081/37
2020-03-01 15:25:55+0000 [-] "127.0.0.1" - - [01/Mar/2020:15:25:55 +0000] "GET /37 HTTP/1.1" 200 126 "-" "curl/7.61.1"
The recommendations for user 37 are [2110:5.0,2243:5.0,2572:5.0,2111:5.0,265:5.0,2375:5.0,594:5.0,3035:5.0,1188:5.0,3168:5.0]
[hadoop@ip-172-31-19-194 redis-2.8.7]$ 
