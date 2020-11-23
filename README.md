# NCU_AI-ML
108 2nd semester


## HW3 QLearning 走迷宮
- 執行結果：77步
  ![image](https://github.com/minyiii/NCU_AI-ML/blob/main/HW3_QLearning/photo/result.png)
- 作法：
  1. 變數值設定如下，ENV_COL、ENV_ROW是環境的column及row數。
    ![image](https://github.com/minyiii/NCU_AI-ML/blob/main/HW3_QLearning/photo/1.png)
  2. 因不知會有多少state，所以Q table中的每row都是動態去append。主要會透過check_state_exist() 來檢查該state是否已存在於Q table中，若不在會自動append新的row，其中每個action值都會預設為0。[參考來源](https://morvanzhou.github.io/tutorials/machine-learning/reinforcement-learning/2-2-tabular-q1/)
    ![image](https://github.com/minyiii/NCU_AI-ML/blob/main/HW3_QLearning/photo/2.png)
    ![image](https://github.com/minyiii/NCU_AI-ML/blob/main/HW3_QLearning/photo/2-2.png)
  3. get_env_feedback() 中，我先計算出下個state (new_S) 才丟入if else去判斷要給何種reward，最終是否會return這個新的state也是在後面if else才去決定。
環境給的reward分為三類。第一種是超過範圍，此類reward為負、S_為舊的state (S)。第二種是找到寶藏，他的reward為5、S_為’terminal’。第三種是剩下的，S_為新的state (new_S)，而因寶藏在右下角，所以reward會根據action不同有所差異；若action是right、down，會給予+1的reward，讓他知道往右或往下走才會找到寶藏，而up、left就沒給reward。
    ![image](https://github.com/minyiii/NCU_AI-ML/blob/main/HW3_QLearning/photo/3.png)
  4. 畫出迷宮有另外寫function。除了包含寶藏T的最後一行有另外處理之外，上面ENV_ROW-1行都用for loop畫出。115行是為了判斷是否為o所處的那row，如果是會把env_list中的他的位置改成’o’，印出後再改回’-‘。跳出迴圈後會再看o是否在最後一行，是的話就會加上它的位置。最後只要再改掉env_list中的最後一個元素為T即可印出。
    ![image](https://github.com/minyiii/NCU_AI-ML/blob/main/HW3_QLearning/photo/4.png)
