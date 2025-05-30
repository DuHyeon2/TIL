![Image](https://github.com/user-attachments/assets/c4318983-db7b-4768-a789-7c33a3fbf220)

# 재색인(Reindex)
- 재색인(Reindex)은 기존 인덱스의 데이터를 새로운 인덱스로 복사하는 작업입니다.
- 이 작업은 데이터 구조를 변경하거나, 새로운 인덱스 설정을 적용할 때 유용합니다.
- 재색인 작업은 Elasticsearch의 `_reindex` API를 사용하여 수행할 수 있습니다.

## 1. 재색인 작업
- 재색인 작업은 다음과 같은 형식으로 수행됩니다.
```json
POST _reindex
{
  "source": {
    "index": "source_index"  # 기존 인덱스 이름
  },
  "dest": {
    "index": "destination_index"  # 새로 생성할 인덱스 이름
  }
}
```

