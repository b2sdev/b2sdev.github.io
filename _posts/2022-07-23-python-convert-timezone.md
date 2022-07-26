
Python에서 단순히 지역 시간을 UTC로 변경하려면 pytz 모듈을 사용하지 않아도 됨을 알게 되었다. 아래가 내가 작성할 수 있는 가장 깔끔한 코드일 것 같다.

```python
from datetime import datetime, timezone

def convert_to_utc(time_str, time_format):
    return datetime.strptime(time_str, time_format) \
        .astimezone(timezone.utc).strftime(time_format)

assert convert_to_utc("2022-07-23 08:31:17",
    "%Y-%m-%d %H:%M:%S") == "2022-07-22 23:31:17"
assert convert_to_utc("2022-07-23 09:31:17",
    "%Y-%m-%d %H:%M:%S") == "2022-07-23 00:31:17"
assert convert_to_utc("2022-07-23 23:31:17",
    "%Y-%m-%d %H:%M:%S") == "2022-07-23 14:31:17"
```

지역 시간에 시간대(timezone) 적용이 필요하거나 UTC를 제외한 시간대로 변환하려면 pytz 모듈을 사용하면 된다. 다만, pytz 모듈의 함수들은 사용하기가 매우 껄끄럽다. 아래 코드는 위의 convert_to_utc 함수를 pytz를 활용하여 구현한 것이다. 변환해야 할 것들이 너무 많다.

```python
from datetime import datetime, timezone
import pytz

time_str = "2022-07-23 08:31:17"
time_format = "%Y-%m-%d %H:%M:%S"

dt_naive = datetime.strptime(time_str, time_format)

local = pytz.timezone("Asia/Seoul")
utc = pytz.timezone("UTC") # pytz.utc와 동일

local_dt = local.localize(dt_naive)
utc_dt = local_dt.astimezone(utc)

print(utc_dt.strftime(time_format)) # -> 2022-07-22 23:31:17
```

이를 원하는 포맷을 반환하는 함수로 구현하려면 4개의 인자를 받아야 한다. 그런데, 아래에서 보면 주어진 시간의 시간대까지 포함된 앞의 3개의 인자가 사실상 한 묶음이고, 마지막 인자는 변환될 시간대로서 앞의 3개의 인자와는 이질적이다. 이걸 다 받아서 하나의 함수로 만들려고 영 마음에 들지 않는 구현이 된다.

```python
def convert_timezone(time_str, time_format, zone_asis, zone_tobe):
    # ...
```

`클로저`를 사용한다면 변환 전과 변환 후의 인자를 각각 받아서 처리할 수는 있다.

```python
def convert_timezone(time_str, time_format, zone):
    dt_naive = datetime.strptime(time_str, time_format)
    local_dt = pytz.timezone(zone).localize(dt_naive)

    def convert(zone):
        tz = pytz.timezone(zone)
        return local_dt.astimezone(tz)

    return convert


converter = convert_timezone(time_str, time_format, "Asia/Seoul")
print(converter("UTC").strftime(time_format)) # -> 2022-07-22 23:31:17
```

여기까지는 좋았는데 막상 적용해보려고 하니 함수가 재사용이 되지 않는다. 정적인 것과 동적인 것의 순서를 바꿀 필요가 있겠다. 인자를 받는 순서를 바꾸어보자.

```python
def convert_timezone(zone):
    tz = pytz.timezone(zone)
    
    def convert(time_str, time_format, zone):
        dt_naive = datetime.strptime(time_str, time_format)
        local_dt = pytz.timezone(zone).localize(dt_naive)
        return local_dt.astimezone(tz).strftime(time_format)

    return convert

convert_to_utc = convert_timezone("UTC")
print(convert_to_utc("2022-07-23 08:31:17", "%Y-%m-%d %H:%M:%S", "Asia/Seoul")) # -> 2022-07-22 23:31:17
```

여기까지 하고 나니 함수가 자유자재로 써먹을 수 있는 꼴이 된 것 같다.

```python
convert_to_korea = convert_timezone("Asia/Seoul")
convert_to_sfo = convert_timezone("US/Pacific")

time_str = "2022-07-22 23:31:17" # UTC
time_format = "%Y-%m-%d %H:%M:%S"

korea_dt = convert_to_korea(time_str, time_format, "UTC")
assert korea_dt == "2022-07-23 08:31:17"

sfo_dt = convert_to_sfo(korea_dt, time_format, "Asia/Seoul")
assert sfo_dt == "2022-07-22 16:31:17"

utc_dt = convert_to_utc(sfo_dt, time_format, "US/Pacific")
assert utc_dt == time_str
```

참고로, Python 3.9에서는 [ZoneInfo](https://docs.python.org/3/library/zoneinfo.html) 모듈이 추가되어 pytz를 대체할 수 있다.

```python
from datetime import datetime
from zoneinfo import zoneinfo

ASIA_SEOUL = ZoneInfo("Asia/Seoul")
utc_tz = ZoneInfo("UTC")

print(datetime \
      .strptime(time_str, time_format) \
      .replace(tzinfo=local_tz) \
      .astimezone(utc_tz) \
      .strftime(time_format)
     )
```