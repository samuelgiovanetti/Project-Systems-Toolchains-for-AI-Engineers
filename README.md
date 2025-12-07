[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/hS-xkOoW)

DISCLAIMER

1. This project was run on a 2024 MacBook Pro M4 with 16 GB of RAM. If your machine does not meet these specifications, I cannot guarantee it will work correctly, and I am not responsible for any issues that may arise.
2. Each run will burn up to 5000 Google API quotas, depending on the time of the day.
3. The cloud version was executed on a virtual machine of type n1-standard-32 (32 vCPUs, 120 GB Memory), equipped with an NVIDIA T4 GPU (16 GB memory) and a 50 GB SSD persistent disk. While such computational power is advantageous and not an issue in an academic environment, it is not strictly required (especially the large amount of memory) as the same task can be executed almost entirely on the local machine, in much more time.

INSTRUCTIONS

1. Clone the repository (Hint: you can use the command: "git clone https://github.com/samuelgiovanetti/Project-Systems-Toolchains-for-AI-Engineers.git")
2. Unzip the Data.zip file you find into the Data folder.
3. Open Project2.ipynb using Jupyter Notebook.
4. Ensure all dependencies are installed. If any are missing, install them. (Hint: Use a virtual environment, Spark can be heinous)
5. Make sure you have enough computational power to fetch the .csv (Spoiler: 20M entries)
6. Download the following [file](https://jdbc.postgresql.org/download/) and position it in your SPARK_HOME directory under jars folder
7. Update the db_properties settings to run the Notebook with your own SQL server settings. Remember to create a schema mqtt.
8. Make sure you have installed the YouTube Data API v3 on your Google cloud project and have your Google cloud key .json file in the same folder of the Jupyter Notebook file.
9. Create a TOPIC on your kafka streaming platform. Have a look [here](https://docs.confluent.io/platform/current/get-started/platform-quickstart.html)
10. Update the BROKER, TOPIC and GOOGLE_APPLICATION_CREDENTIALS based on your ones.
11. Create a virtual machine on Google Cloud (or any other cloud system), install Docker, set up and launch a PostgreSQL container, and then return to step 7.
12. Run the [notebook](Project2.ipynb) step by step. Run [Task_IV_Project2_cloud.ipynb](Task_IV_Project2_cloud.ipynb) for Task 4 and the cloud version.
13. If you have any issue, check out this video [here](https://cmu.box.com/s/y2b2murl4igusvzxzv6437px2he622dr)
14. If you still have issues, contact me at: sgiovane@andrew.cmu.edu
   
SCHEMA/DESCRIPTION:

The dataset is composed by IoT sensors based on MQTT where each aspect of a real network is defined. In particular, the MQTT broker is instantiated by using Eclipse Mosquitto and the network is composed by 8 sensors. The scenario is related to a smart home environment where sensors retrieve information about temperature, light, humidity, CO-Gas, motion, smoke, door and fan with different time interval since the behaviour of each sensor is different with the others. 
(https://www.kaggle.com/datasets/cnrieiit/mqttset)

The network traffic includes both TCP protocol information such as flags, timing, and packet length, as well as MQTT features that track message types, connection flags, acknowledgments, quality of service, topics, and other protocol parameters. The dataset also includes control messages that establish and maintain connections alongside publish/subscribe message exchanges.

Following the schema of the dataset used.
<pre>
root
 |-- tcp.flags: string (nullable = true)
 |-- tcp.time_delta: double (nullable = true)
 |-- tcp.len: integer (nullable = true)
 |-- mqtt.conack.flags: string (nullable = true)
 |-- mqtt.conack.flags.reserved: double (nullable = true)
 |-- mqtt.conack.flags.sp: double (nullable = true)
 |-- mqtt.conack.val: double (nullable = true)
 |-- mqtt.conflag.cleansess: double (nullable = true)
 |-- mqtt.conflag.passwd: double (nullable = true)
 |-- mqtt.conflag.qos: double (nullable = true)
 |-- mqtt.conflag.reserved: double (nullable = true)
 |-- mqtt.conflag.retain: double (nullable = true)
 |-- mqtt.conflag.uname: double (nullable = true)
 |-- mqtt.conflag.willflag: double (nullable = true)
 |-- mqtt.conflags: string (nullable = true)
 |-- mqtt.dupflag: double (nullable = true)
 |-- mqtt.hdrflags: string (nullable = true)
 |-- mqtt.kalive: double (nullable = true)
 |-- mqtt.len: double (nullable = true)
 |-- mqtt.msg: string (nullable = true)
 |-- mqtt.msgid: double (nullable = true)
 |-- mqtt.msgtype: double (nullable = true)
 |-- mqtt.proto_len: double (nullable = true)
 |-- mqtt.protoname: string (nullable = true)
 |-- mqtt.qos: double (nullable = true)
 |-- mqtt.retain: double (nullable = true)
 |-- mqtt.sub.qos: double (nullable = true)
 |-- mqtt.suback.qos: double (nullable = true)
 |-- mqtt.ver: double (nullable = true)
 |-- mqtt.willmsg: double (nullable = true)
 |-- mqtt.willmsg_len: double (nullable = true)
 |-- mqtt.willtopic: double (nullable = true)
 |-- mqtt.willtopic_len: double (nullable = true)
 |-- target: string (nullable = true)
 |-- dataset: string (nullable = true)
 |-- id: long (nullable = true)
</pre>
   
 TASK 1:
 
 There are many nested features making NoSQL databes an ideal candidate. This would allow to better describe the intrinsic structure of the data, adding some features as attributes of other core features. Not sure Neo4J free subscription would allow 20M entries, but generally a NoSQL database would be better suited than a relational one, in this case.

TASK 2.4:

<img width="567" height="455" alt="Histo_combined" src="https://github.com/user-attachments/assets/6da084c2-b508-44cf-90f9-5dd5303e4e3f" />
<img width="567" height="455" alt="Histo_test" src="https://github.com/user-attachments/assets/a0f1ed68-5948-4b2b-831e-2c3982829736" />
<img width="554" height="455" alt="Histo_train" src="https://github.com/user-attachments/assets/772ecdbe-4896-4d3c-be04-bb92f2c667f6" />
<br><br>

TASK 3:

LOGISTIC REGRESSION:

As a first natural classifier, I chose the logistic regression model. I started with this model because of its simplicity and to assess whether the features shows a complex or rather linear pattern. Regarding the parameters, I focused on the number of maximum iterations and the regularization parameter, which limit the complexity of the function, as based on experience they are the most effective in influencing the model’s outcome.  

On the local version, I started with maxIter = 200 and regParam = 0.001. Following a trial-and-error strategy aimed at improving accuracy, I gradually decreased the maximum number of iterations by 50 at each trial until reaching maxIter = 50. For the regularization parameter, I decreased regParam to 0.0001, noticing a slight increase in accuracy. Generally, although these two parameters should be the most effective in altering the model’s predictions, the changes produced only minor differences in performance, with accuracy fluctuating between 66% and 68%.  

On the cloud version, I was able to systematically iterate through the ParamGridBuilder using the following parameters: maxIter: and regParam: [0.0001, 0.001, 0.01, 0.1]. The results confirmed that maxIter = 50 and regParam = 0.0001 were the ideal parameters, corroborating my earlier findings. 
Initially, I also tested other parameters such as elasticNetParam and fitIntercept, but changing these values did not noticeably affect the model’s predictions. Therefore, I decided to simplify and retain only maxIter = 50 and regParam = 0.0001.  

While I could have experimented with even lower values for maxIter and regParam, this would likely have oversimplified the model and not improved the results. 
Given the unsatisfactory performance, it seems that logistic regression is not well-suited to capture the more complex patterns in the data, as it remains too simplistic.

<br>
SPARK XGBClasifier:

Given the experience with the logistic regression model, it became evident that a more complex approach was needed to better capture the underlying patterns in the data. As a natural next step, I initially considered a Random Forest classifier, given the multiclass nature of the problem. However, after testing an initial version of the Random Forest model on just 0.1% of the dataset, it took approximately 48 minutes to produce slightly better results than logistic regression (around 83% accuracy). Considering the significant training time and the goal of eventually fitting the model on the full dataset, this approach did not seem justifiable.

Therefore, I began exploring ways to leverage the scalability capabilities offered by Spark. Inspired by the [paper](https://arxiv.org/pdf/2408.00480) and after consulting the [official documentation](https://xgboost.readthedocs.io/en/latest/tutorials/spark_estimator.html), I decided to experiment with the SparkXGBClassifier. Observing its conceptual similarities to Random Forest, I selected as initial parameters max_depth=20, n_estimators=200, and tree_method='hist', along with the intrinsic features_col and label_col settings specific to the dataset. Additionally, I set num_workers=10 to exploit the 10 cores available on my local machine and enable parallel computation.

During parameter adjustment, I left tree_method mostly unchanged, as it is considered the most suitable option for large datasets. Instead, I focused on tuning the maximum depth and the number of estimators, manually adjusting these parameters and evaluating the performance at each step. Treating the classifier similarly to a Random Forest model, I sought to balance complexity and performance. I gradually reduced n_estimators by increments of 50, testing values of 200, 150, 100, and 50, and observed virtually no difference in accuracy, which consistently remained around 90% (with only minor decimal fluctuations). I then experimented with smaller values, n_estimators=30 and n_estimators=10, before returning to n_estimators=20, at which point the accuracy began to decrease. Following the principle of preferring simpler models when performance is equivalent, I selected the lower number of estimators.

Similarly, I tested different tree depths, max_depth=10, max_depth=5, and max_depth=3, eventually returning to max_depth=5, which maintained accuracy while reducing complexity. The final configuration, with max_depth=5 and n_estimators=20, achieved an accuracy of 90.28%, compared to a reported upper bound of around 95% in the aforementioned paper.

On the cloud version, which provided more computational resources, I increased num_workers to 32, obtaining much faster results while keeping the other hyperparameters unchanged. I also tested GPU acceleration by setting tree_method='gpu_hist', but this unexpectedly led to a significant drop in accuracy (to about 22%), likely due to configuration issues. Given the strong performance of the 'hist' method, I decided not to pursue GPU optimization further. 

Finally, I experimented with additional parameters such as learning_rate and reg_lambda, but these resulted in lower accuracy. Consequently, I simplified the model and excluded them from the final configuration.

<br>
SHALLOW NEURAL NETWORK

For the second part of the task, I decided to fully experiment with PyTorch, testing both a shallow and a deep neural network to evaluate their differences. 
I began with a shallow neural network consisting of 3 linear layers (plus 2 activation layers) with the following structure:

```
self.layers = nn.Sequential(
   nn.Linear(input_size, 16),
   nn.ReLU(),
   nn.Linear(16, 8),
   nn.ReLU(),
   nn.Linear(8, output_size)
)
```
        
In this case, I did not tune the number of layers extensively: increasing them would likely have turned the network into a deep one, while decreasing them would have oversimplified the structure. Therefore, I started with 3 linear layers, which I largely kept unchanged. Regarding the activation function, I kept the default choice of ReLU, which is particularly suitable for shallow networks since the gradient is unlikely to vanish over such a small number of layers.

Regarding batch size, for the local version I started with 64, then increased it to 128 to reduce the slight noise observed in the gradients. For the cloud version, batching was not strictly necessary given the available GPU resources; however, to balance GPU utilization and gradient noise, I chose a batch size of 64,192 (doubling progressively). This allowed the GPU to be effectively exploited while avoiding too little gradient noise, which could have led to convergence to local minima. 

The number of epochs was chosen to balance computational efficiency and performance. On the local version, I experimented with 10, 20, and 40 epochs, allowing the loss and accuracy to stabilize without spending excessive time. For the cloud version, given the available resources, I set 150 epochs. 

For the optimizer, I chose Adam, starting with a low learning rate of 0.00001 and gradually increasing it to 0.001 to allow exploration of the parameter space and prevent premature convergence. This strategy produced a noticeable improvement in accuracy. I set weight_decay=1e-4, which I had tuned between 1e-3 and 1e-5 without any noticeable change, so the initial value was retained. Other optimizers, such as AdamW, were not used since regularization was not a limiting factor in this case. 

For the loss function, I used CrossEntropyLoss, a standard choice for multi-class classification due to its simplicity and numerical stability. Given the nature of the dataset, no class weighting appeared necessary. 

Regarding the learning rate scheduler, I initially chose ExponentialLR. While it was a reasonable choice for the local version with fewer epochs, it may not have been optimal for the cloud version, as the model converged and stabilized too early over the large number of epochs. I also experimented with ReduceLROnPlateau, which was a good candidate but introduced excessive fluctuations in learning rate adjustments.

<br>
DEEP NEURAL NETWORK

In the deep neural network experiment, I increased both the number of layers and the number of neurons per layer to introduce additional complexity, aiming to capture more intricate patterns. Initially, I started with very large values—512 neurons per layer and 10 layers—but gradually reduced these to the current structure, as the extra complexity did not translate into better performance. 
Eventually, I settled on the following architecture:

```
self.layers = nn.Sequential(
   nn.Linear(input_size, 256),
   nn.ReLU(),
   nn.Linear(256, 128),
   nn.ReLU(),
   nn.Linear(128, 64),
   nn.ReLU(),
   nn.Linear(64, 32),
   nn.ReLU(),
   nn.Linear(32, output_size)
)
```

The number of epochs was chosen using the shallow network as a benchmark: I doubled the epochs compared to both the local and cloud shallow networks, which proved to be a reasonable choice.

For the other hyperparameters (optimizer, learning rate, weight decay, loss function, scheduler), I followed the same structure as in the shallow model to maintain a balance and avoid introducing large discrepancies relative to the shallow network. This decision was also supported by the fact that the deep network, while slightly larger, did not drastically increase complexity. 

 <br><br>
 DEMO VIDEO:
 
 https://cmu.box.com/s/y2b2murl4igusvzxzv6437px2he622dr
