# バージョンは 3.6 以上

```bash
python --version #3.11

pip3 install fastapi
pip3 install uvicorn

```

# 起動方法

uvicorn main:app --reload

# 本番公開

Python 用 ASGI Web サーバ実装「 Uvicorn 」が必要

# デコレーター

@app ~　： デコレーターと呼びます

# API ドキュメントの自動生成

Swagger UI: ベースでブラウザー上でみやすく表示してくれるツール

▼Swagger UI ベース

http://127.0.0.1:8000/docs

▼HTML ベース

http://127.0.0.1:8000/redoc

# パスパラメータ

```bash
# パスパラメータ
# 変数として保存して
# 関数内の処理で用いることができる
@app.get('/countries/{country_name}')
async def country(country_name):
  return {'country_name': country_name}


```

# 必須ではないオプションパラメータの設定

```
Optional[str] = None , country_no: Optional[int] = None
```

# リクエストボディ

class 構文を使用してデータ構造を定義する

```bash
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel

# リクエストボディ

# データ構造
class Item(BaseModel):
  name: str
  description: Optional[str] = None
  price: int
  tax: Optional[float] = None

# インスタンス
app = FastAPI()

# リクエストボディ
# 先にデータ構造を設定する（定義の方法）
@app.post('/item/')
async def create_item(item: Item):
  return {'message': f'{item.name}は、税込価格{int(item.price*item.tax)}円です。'}
```

# python で api を叩く

```bash
import requests
import json

def main():
  url = 'http://127.0.0.1:8000/item/'
  body = {
    "name": "T-shirt",
    "description": "string",
    "price": 5980,
    "tax": 1.1
  }
  res = requests.post(url, json.dumps(body))
  print(res.json())

if __name__ == '__main__':
  main()

```

## 入れ子構造になっているリクエストボディ

```bash
from typing import Optional, List
from fastapi import FastAPI
from pydantic import BaseModel

# リクエストボディ

class shopInfo(BaseModel):
  name: str
  location: str

# データ構造
class Item(BaseModel):
  name: str
  description: Optional[str] = None
  price: int
  tax: Optional[float] = None

class Data(BaseModel):
  shop_info: Optional[shopInfo] = None
  items: List[Item]

# インスタンス
app = FastAPI()

# リクエストボディ
# 先にデータ構造を設定する（定義の方法）
@app.post('/')
async def index(data: Data):
  return {"data" : data }
```

## バリデーション

```bash
from pydantic import BaseModel, Field

class Item(BaseModel):
  name: str = Field(min_length=4, max_length=12)
  description: Optional[str] = None
  price: int
  tax: Optional[float] = None

```

## デプロイ

Render : https://dashboard.render.com/
