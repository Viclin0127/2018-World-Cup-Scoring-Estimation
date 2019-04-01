# 2018-World-Cup-Scoring-Estimation (2018世足賽比分預測)

1. using poisson distribution to estimate score ( with python / jupyter notebook)

2. draft code is as below:

```python
from scipy.stats import poisson
import pandas as pd
from scipy.stats import mode
import numpy as np

team_strength = pd.read_excel('球隊參數-4強.xlsx')

# 每一場球賽都會給予一個poisson變數，次數越多隨機因子越小
n_sim = 5

def simulate_match(team_A, team_B, knockout=False):
    # simulating a match, and return home scoring and away scoring #
    
    # first step, grab Goal Rate and Loss Rate
    home_scoring_strength = (team_strength.loc[team_A, '進球率'] + \
                             team_strength.loc[team_B, '失球率']) / 2
    away_scoring_strength = (team_strength.loc[team_A, '失球率'] + \
                             team_strength.loc[team_B, '進球率']) / 2
    # simulate n times
    fs_A =mode(poisson.rvs(home_scoring_strength, size=7))[0][0]
    fs_B =mode(poisson.rvs(away_scoring_strength, size=7))[0][0]
    print(fs_A,fs_B)
    
    return fs_A, fs_B

# start estimate 2018 world cup schedule

schedule = pd.read_excel("對戰表-4強.xlsx")
a_team=schedule["主隊"]
b_team=schedule["客隊"]
score=[]
for a,b in zip(a_team,b_team):
    score.append(simulate_match(a,b,knockout = False))
```
