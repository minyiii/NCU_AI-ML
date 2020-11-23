# NCU_AI-ML
108 2nd semester

## HW2 RNN 股價預測
### 需求
預測南亞電路(8046)的股價，並以產業類別相關的欣興電子(3037)之歷史資料集(3037_2010_2019_csv.csv)來輔助預測

### 結果
#### 作法
1. 利用前20天(time_step=20)的5種資料(input_size=5)來預測最後一天的收盤價(output_size=1)。
2. 5種資料分別是：8046的open、high、low、close、以及3037的close。
3. 一開始是以每time_step天的資料為單位append進data中，之後再從data去切分訓練和測試集。
#### 變數解釋
1. get_next_batch()
> 參考了網路上教學的做法，我覺得這個取法對我來說比較直觀。訓練時會再用它動態抓取1 batch所需的資料來當作X、Y。
2. index_in_epoch
> 為 global variable，用來紀錄下次要開始取的訓練集的index，每次抓完一batch的資料都會再讓它加上batch_size，直到index值超過訓練集大小(代表一個epoch結束)也會重新給值。
3. perm_array
> 它是長度與訓練集相同、內容分別是0~訓練集長度-1、且順序是shuffle過的array，它的用途是用來當作從訓練集取出資料的index順序。每個epoch結束後也會重新shuffle一次，這樣就不會每個epoch的同個batch都抓固定的那batch_size筆的資料去train，目的是降低overfitting的可能性。

## HW3 QLearning 走迷宮
### 需求
以Q-Learning在二維空間中進行尋找寶藏遊戲，空間設為30*40(x=30、y=40)，動作為: 上、下、左、右，寶藏放在右下角的位置，探索者起始點設在左上角。
### 結果
- 最低步數：77步
- 作法：
  1. 變數值設定如下，ENV_COL、ENV_ROW是環境的column及row數。
  2. 因不知會有多少state，所以Q table中的每row都是動態去append。
     主要會透過check_state_exist() 來檢查該state是否已存在於Q table中，若不在會自動append新的row，其中每個action值都會預設為0。
     [參考來源](https://morvanzhou.github.io/tutorials/machine-learning/reinforcement-learning/2-2-tabular-q1/)
  3. get_env_feedback() 中，我先計算出下個state (new_S) 才丟入if else去判斷要給何種reward，最終是否會return這個新的state也是在後面if else才去決定。
     環境給的reward分為三類：
     1. 超過範圍，此類reward為負、S_為舊的state (S)。
     2. 找到寶藏，他的reward為5、S_為’terminal’。
     3. 剩下的，S_為新的state (new_S)，而因寶藏在右下角，所以reward會根據action不同有所差異；若action是right、down，會給予+1的reward，讓他知道往右或往下走才會找到寶藏，而up、left就沒給reward。
  4. 畫出迷宮有另外寫function。除了包含寶藏T的最後一行有另外處理之外，上面ENV_ROW-1行都用for loop畫出。
     115行是為了判斷是否為o所處的那row，如果是會把env_list中的他的位置改成’o’，印出後再改回’-‘。跳出迴圈後會再看o是否在最後一行，是的話就會加上它的位置。最後只要再改掉env_list中的最後一個元素為T即可印出。
