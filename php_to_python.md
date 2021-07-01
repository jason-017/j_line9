### API 데이터 수집 코드 변환(php -> python)
- 소스 코드 시나리오<br>
  - <b>수집데이터:</b> 아파트 실거래가 API in 공공데이터포털
  - <b>API params:</b> ServiceKey(인증키), pageNo(페이지위치), numOfRows(페이지당데이터수), DEAL_YMD(년월), LWAWD_CD(시군구코드)
  -  sggCode는 land_region_code 테이블에 쿼리 날려서 수집
  - URL sample: http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev?ServiceKey=&DEAL_YMD=201906&LAWD_CD=11110 
  - 실거래 가격 변동은 3개월 안에 신고(취소 및 등록) 가능하기 때문에 매번 업데이트마다 <b>기존 3개월치 데이터 삭제</b>하고 <b>새로운 데이터 삽입 필요</b>
  - 1월, 2월의 경우 전년도 데이터를 수집해야 하기 때문에 별도 작업 필요
  - <b>삽입 테이블명:</b> land_trading_detail_sggCode, sggCode에 시군구 코드 넣어주면 된다.
  - DB 칼럼 순서와 API 칼럼 순서가 다를 수 있으므로 정확히 매칭 후 삽입 진행 -> df로 먼저 다 쌓은 후, DB순서에 맞게 변환 + insert 해주자.
  - bulk insert query는 변수(칼럼) 및 bulk 원하는 데이터 수 동적으로 맞출 수 있게 진행, 또한 bulk의 경우 execute에 변수 넣어줄 때 tuple 하나로 넣어줘야 하기 때문에 flatten(1차원) 작업 필수!

bulk insert sample code:<br>
```python 
val = [[1,2,3], [4,5,6]]
val = tuple(sum(val, [])) # print(val)할 경우, (1, 2, 3, 4, 5, 6) 출력
SQL_INSERT = "INSERT INTO TABLE VALUES (%S, %S, %S), (%S, %S, %S);"
execute(SQL_INSERT, val)
```
