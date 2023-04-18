<main align="center">
  <h1 align="center" font-color="red">팀 프로젝트 A</h1><br/>

  <p align="center">
  위 프로젝트는 주어진 주식관련 API를 활용하여 각각의 종목 주가를 표기하며 매도와 매수를 돕기위한 프로젝트로 기획 되었습니다.<br/>
  조금더 자세한 프로젝트 SQL의 위치는 <a href="https://classroom.google.com/c/NTM2OTk2MDc5MTQ4/m/NTA0MzQyMzUxMjMz/details">에 존재합니다.
  <br/>

  <img src="./project-a API 정보.png" style="width: 600px; height: 350px;">
</main>


## core function
- 저장되어 있는 API 주식의 상승과 하락 등의 주가를 View로 출력
- 평균값의 분석과 이를 토대로한 종목추천 기능
- Kospi와 Kosdak의 분리 출력

## Prerequisites
- SQL 형식의 데이터로 저장된 API의 정보가 개개인의 MARIA DB에 저장되어있는 문제가 있음
- node.js의 사용으로 편의성을 높였지만 많은양의 Package파일들이 존재