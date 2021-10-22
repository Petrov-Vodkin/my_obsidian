2021-01-22 13:58
#parametrize #параметризация
#  Параметризация 
Пример 
``` python
num = [i for i in range(10)]  # список\словарь с параметрами
num[7] = pytest.param("7", marks=pytest.mark.xfail)  # перезаписываем параметр со значением 7 (списка прод номером 7) 
@pytest.mark.parametrize("num_promo", num)  # (название пар-ра, откуда берём)
def test_guest_can_add_product_to_basket(browser, num_promo): 
```

#### Links
[[PyTest]] [[Python]] [[Xfail_Skip]] [[Fixtures]]