# 데이터포맷 XML

### JSON과 XML 비교

```json
{
  "CSList" :[{
   "name" : "OS",
   "level" : 5,
  },
  {
   "name" : "Network",
   "level" : 3,
  }
 ]
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CSList>
 <CS>
  <name>OS</name> <level>5</level>
 </CS>
 <CS>
  <name>Network</name> <level>3</level>
 </CS>
</CSList>
```

- XML은 중괄호가 아닌 열고 닫는 태그로 이루어진 구조의 데이터를 의미한다.
- 맨위 버전, 인코딩, utf-8의 설정을 프롤로그라고 한다.
- 최상위 태그는 하나만 사용가능하다.
- JSON과 비교해서 닫힌태그가 반복되서 무겁고 변환하기 힘들다.

### Sitemap.xml

- xml이 대표적으로 사용되는곳이 sitemap.xml이다.
- 사이트가 크거나 서로 링크가 종속적이지 않을때 크롤러가 페이지를 누락하지 않도록 사이트맵을 사용한다.
