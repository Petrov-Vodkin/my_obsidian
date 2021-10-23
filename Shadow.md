2020-12-14 13:17
#тень #shadow
# Shadow
#### Суть
 ***Вход***
 тень ближе к цене открытия, чем на 5% ATR (протестить) 
 входить на минимальном расстоянии ABS(sh_sell-sh_buy)(протестить)
Фильтры: 
sell : low_0 > low_1 || sell : high_0 < high_1

#### Суть
***Вход***: 
Sh sell > (выше) Keltner Canal 3d ==> sell
Sh buy < (ниже) Keltner Canal 3d ==> buy

#### Stop loss
if buy &&  price < sh.sell && sh.sell < ma3day : stop

#### Links
