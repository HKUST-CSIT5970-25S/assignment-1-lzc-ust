[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: LI, Zhicheng
### Student Id: 21133094
### Email: zlijw@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Your answer goes here.
    1. **Measurement Tool**:
    
       - **Name**: **Sysbench**, a popular open-source benchmarking tool that can measure CPU and memory performance.
    
    2. **Configuration of the Measurement Tool**:
    
       - CPU Performance:
         - Command: `sysbench cpu --cpu-max-prime=20000 run`
         - **Explanation**: The `--cpu-max-prime` parameter sets the maximum prime number up to which the CPU performance will be tested. A higher value increases the workload, providing a more comprehensive performance measurement.
       - Memory Performance:
         - Command: `sysbench memory --memory-block-size=1M --memory-total-size=10G run`
         - **Explanation**: The `--memory-block-size` parameter sets the size of the memory blocks to be allocated, and `--memory-total-size` sets the total amount of memory to be tested. These values ensure that the memory performance is tested under significant load.
    
    3. **Explanation of Measurement Results**:
    
       - CPU Performance:
    
         - The result will include metrics like the total execution time and the number of events per second. These values represent how efficiently the CPU can handle computational tasks.
    
         ![CPU](https://github.com/user-attachments/assets/f9b238c4-3b4b-42d8-8f27-131910e1926f)

    
       - Memory Performance:
    
         - The result will include metrics like the total amount of transferred data and the transfer rate. These values indicate the speed and efficiency of memory operations.
    
         ![memory](https://github.com/user-attachments/assets/c8c89bfb-ee87-4ff0-bdce-d8c11ad244fc)


3. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    **Analyze Performance Differences**

    - CPU Performance:
      - t2.micro and t2.medium have similar CPU performance, with 881.73/s and 887.67/s respectively.
      - c5d.large has lower CPU performance at 476.97/s, which might be due to its optimization for specific high-performance computing tasks rather than general CPU benchmarks.
    - Memory Performance:
      - t2.micro and t2.medium have similar memory performance, with 19018.10/s and 19075.94/s respectively.
      - c5d.large shows significantly higher memory performance at 20782.97/s, indicating better efficiency in memory operations.

    **Conclusion**

    - **CPU Performance**: The performance of t2.micro and t2.medium instances is similar, while c5d.large shows lower performance in this specific CPU benchmark.
    - **Memory Performance**: c5d.large demonstrates significantly higher memory performance compared to t2.micro and t2.medium.
  
    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |     881.73/s      |      19018.10/s      |
    | `t2.medium`  |   887.67/s      |      19075.94/s      |
    | `c5d.large` |    476.97/s    |    20782.97/s       |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |     10200      |  1.127   |
    | `m5.large` - `m5.large`   |     49700      |  0.298   |
    | `c5n.large` - `c5n.large` |     49600      |  0.289   |
    | `t3.medium` - `c5n.large` |     10200      |  0.774   |
    | `m5.large` - `c5n.large`  |     49600      |  0.707   |
    | `m5.large` - `t3.medium`  |     10200      |  1.106   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    **Analysis:**

    - **TCP Bandwidth**:
      - `m5.large` and `c5n.large` instances show significantly higher TCP bandwidth compared to `t3.medium` instances.
      - The bandwidth between `m5.large` and `c5n.large` instances is almost the same, indicating similar network performance capabilities.
    - **Round-Trip Time (RTT)**:
      - `m5.large` and `c5n.large` instances have much lower RTT compared to `t3.medium` instances, indicating faster response times.
      - The RTT between instances of the same type is generally lower than between different types, except for `t3.medium` instances which have higher RTT overall.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |       504      |  62.866  |
    | N. Virginia - N. Virginia |      49700     |  0.217   |
    | Oregon - Oregon           |      95300     |  0.102   |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    **Analysis:**

    - **TCP Bandwidth**:
      - The TCP bandwidth between instances in the same region (N. Virginia - N. Virginia and Oregon - Oregon) is significantly higher compared to instances in different regions (N. Virginia - Oregon).
      - The highest bandwidth is observed between instances in Oregon, followed by N. Virginia.
    - **Round-Trip Time (RTT)**:
      - The RTT is much lower for instances within the same region compared to instances in different regions.
      - The RTT between N. Virginia and Oregon is considerably higher, indicating longer latency due to the geographical distance.
    
    **Conclusion:**
    
    - **Intra-region performance**: Instances within the same region exhibit high TCP bandwidth and low RTT, indicating efficient and fast network performance.
    - **Inter-region performance**: Instances across different regions show lower TCP bandwidth and higher RTT, reflecting the impact of geographical distance on network performance.
