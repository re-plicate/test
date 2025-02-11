# DynaGraph

## model run
user could run main.py to easily start the training.
test.py is for evaluating the DynaGraph.

## 1.Dataset
This project includes 3 widely used datasets PeMS04, PeMS08, and a new added large-size dataset Q-Traffic for reproducing the results. Regarding the Q-Traffic dataset, it consists of three sub-datasets: query sub-dataset, traffic speed sub-dataset and road network sub-dataset. We conducted experiments on the traffic speed sub-dataset.

### Traffic Speed Sub-dataset on Q-Traffic dataset
This sub-dataset was collected in Beijing, China between April 1, 2017 and May 31, 2017, from the [Baidu Map](https://map.baidu.com). This sub-dataset contains 15,073 road segments covering approximately 738.91 km. 
They are all in the 6th ring road (bounded by the lon/lat box of <116.10, 39.69, 116.71, 40.18>), which is the most crowded area of Beijing. The traffic speed of each road segment is recorded per minute. To make the traffic speed predictable, for each road segment, original authors use simple [moving average](https://en.wikipedia.org/wiki/Moving_average) with a 15-minute time window to smooth the traffic speed sub-dataset and sample the traffic speed per 15 minutes. Thus, there are totally 5856 (61×24×4) time steps, and each record is represented as road_segment_id, time_stamp ([0, 5856)) and traffic_speed (km/h).

There are some traffic speed samples as follows:

```
15257588940, 0, 42.1175  

..., ..., ...  
  
15257588940, 5855, 33.6599  

1525758913, 0, 41.2719  

..., ..., ...  
```



## 2.Preliminary Comparison with the newly referred algorithms
Due to some methods have no open-source code, it is hard to reproduce and are marked with the marker *.

*represents the result is not fiiled by now but will be continuously updated

After chosen the perform better baselines, we will added them in our scalability evaluation of the TAXIBJ dataset and newly added larger dataset.

![image](https://github.com/re-plicate/DynaGraph/blob/main/Fig/Tab1.png)

The results shown in Table 1 demonstrate the comparable performance of DynaGraph. DynaGraph outperforms the baselines except for the Detectornet. However, this baseline has no open-source code and we refer to the results from the initial paper. Due to the GMSDR, ST-GDN, and DGCRN having no easily open-source code to implement, to fairness, we temporarily fill in no results on these blanks.

![image](https://github.com/re-plicate/DynaGraph/blob/main/Fig/Tab2.png)

The results in Table 2 compare DynaGraph with all the open-source baselines. We average the metrics of the 12 steps prediction results, what method is used by most of the refereed papers, to form this table. It is surprising that DynaGraph achieves SOTA in both two datasets. STAWnet shows the closest performance of DynaGraph, which has some similar mechanisms to DynaGraph.
<!-- 
Worth mentioning, LSTF-LINEAR is a linear model proposed by [AAAI2023] "Are Transformers Effective for Time Series Forecasting". We pose it to see what will happen and if it can verify the hypothesis that "former structures may not be as good as linear layers in long-term time series forecasting task" mentioned in the paper. From the surface of the results, this hypothesis does not seem to fit the traffic prediction dataset. However, we will try to discover deeper in the future to find out the feasibility and limitation of the proposed hypothesis.
-->


## 3.Time cost on TAXIBJ dataset
<img width="887" alt="1685357763960" src="https://github.com/re-plicate/DynaGraph/blob/main/Fig/Fig123.jpg">

<p align="center">Results on TAXIBJ dataset</p>
  
 Figure a: One epoch training time. 

Figure b: Total training time results. We set the bacthsize of each model as 8 and record the time cost for final convergence. We define the final convergence of each model as that the accuracy is no longer improved in the next 10 epochs of learning, then we record the time cost between the training start and the best accuracy emerges as the total training time.

Figure c: Prediction latency. We start timing at the beginning of the data processing and stop timing when the entire test data set is predicted. Our DynaGraph has a multi-scale temporal features fusion module which cost more time than other methods in the data processing procedure. However, the overall prediction latency is acceptable and its growth tendency is smoother.

## 4.Preliminary results of Q-Traffic dataset
we have conducted a preliminary scaled evaluation with DynaGraph on Q-traffic dataset. However, this evaluation needs some time and more computional resource to produce the performance results of DynaGraph or other baselines. Here, we successfully run our model on Q-traffic dataset and what do we need is just time to produce the results. Here we present the recorded results which will be continuously updated.

<!-- ![image](https://github.com/re-plicate/DynaGraph/assets/130746797/f9a7768f-e4e5-4f2f-92b9-309b3a3b927a) -->
<img width="887" alt="1685357763960" src="https://github.com/re-plicate/DynaGraph/blob/main/Fig/Tab33.png">

<p align="center">Results on Q-Traffic dataset</p>

We compared DynaGraph with STAWnet and DGCRN, the results show that DynaGraph has a more stable accuracy with the increasing scalability. DynaGraph shows a better performance on longer time prediction.

Additionally, more details of DynaGraph are coming soon.



<img width="587" alt="1685357763960" src="https://github.com/re-plicate/DynaGraph/blob/main/Fig/qt_time.jpg">

<p align="center">Total training time on Q-Traffic</p>

Figure shows the one epoch training time cost and total training time cost among 10%-35% scalability. The results reveal the linear growth of time cost on the larger-size dataset (~15k road segments and ~45k data channels), which is acceptable. Overall scalability results on this dataset will be updated continuously.

