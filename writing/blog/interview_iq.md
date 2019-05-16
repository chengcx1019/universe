> 智力和概率，持续更新，面试不息，更新不止

## 概率题

1. 连续的网络流中等概率的抽取一个数据包

   记录当前到达的数据包总数n，以1/n的概率抽取当前数据包假设n-1时成立，即前n-1个数据包的概率都是1/n-1，当前正在读取第n个数据包，以1/n的概率抽取它，那么前n-1个数据包的抽取概率为n-1/n*1/n-1

   **更一般的将问题泛化为取k个数据包**，问题描述： 
   取前k数据包个元素放入缓存池中。从i=k+1开始，以k/i的概率取第i个元素。若第i个元素被选中，已均等的概率替换蓄水池中的先前被选中的任一元素。 

   证明：

   当 i = k+1时，前k个数据包被选中的概率为1-k/(k+1)*1/k=k/(k+1)

   假设当i=n时，前n个数据包被抽取的概率是k/n

   那么第i=n+1时，第n+1个数据被抽取的概率是k/(n+1)，那么前n个数据包中每个数据包被抽中分两种情况，1+n未被抽中，1+n被抽中但该数据包未被替换

   $\cfrac{k}{n}*(1-\cfrac{k}{n+1})+\cfrac{k}{n}\cfrac{k}{n+1}(1-\cfrac{1}{k})=\cfrac{k}{n+1}$,i=n+1时满足条件，假设成立



   为帮助理解，另附python模拟该过程

   ```python
   import random, collections
   def reservoir(n, k):
       """
       n:数据流长度
       k:抽取的数据包长度
       """
       packets = [i for i in range(n)]
       reservoir = packets[0:k]
       for i in range(k, n):
           M = random.randint(1, i+1)
           if M <= k:
               j = random.randint(0, k-1)
               reservoir[j] = packets[i]
       return reservoir
   
   def run(rounds, stream_len, k):
       dic = collections.defaultdict(int)
       for _ in range(rounds):
           rets = reservoir(stream_len, k)
           for ret in rets:
               if ret not in dic:
                   dic[ret] = 1
               else:
                   dic[ret] += 1
   
       for k,v in dic.items():
           print(k, v)
   run(10000, 10, 3)  # 则0-9被抽样的数目差不多均等于3000次
   ```



   ## 智力题

   1. 监狱囚犯放风

      问题描述：一个牢房关有100个囚犯，每次随机挑选一名囚犯出去牢房外的院子放风，院子里有一盏灯，并且有一个开关可以控制灯亮或者灭，约定如果在某一次放风结束，可以确定100人都已出去放风过，则全部人都可以获得释放，否则就会被判处终身监禁，请问有什么策略可以使100人释放。（开始前这100人事先可以聚集在一起商量对策，之后被关进各自牢房，不得再接触交流，可以随机抽取无数次）

      解答：约定由一个人进行计数，每个人第一次出去才可以打开灯，只有计数的人可以关灯，且每次关灯计数加1，待此人计数满100次后即可。