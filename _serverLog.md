---


---

<hr>
<h2 id="title-serverlogdescriptiondate-2021-10-03t2321220900draft-false">title: “ServerLog”<br>
description:<br>
date: 2021-10-03T23:21:22+09:00<br>
draft: false</h2>
<h2 id="v01-20172020.11">v01 (2017~2020.11)</h2>
<h3 id="사진">사진</h3>
<p><img src="page/serverLog/01-01.png" alt="1 세대"><br>
<img src="page/serverLog/01-01.png" width="50%" height="50%"></p>
<h3 id="제원">제원</h3>

<table>
<thead>
<tr>
<th>총 금액</th>
<th>~60,000원</th>
</tr>
</thead>
<tbody>
<tr>
<td>기종</td>
<td>라즈베리파이3</td>
</tr>
<tr>
<td>OS</td>
<td>라즈비안</td>
</tr>
<tr>
<td>CPU</td>
<td>4Core 1.2GHz</td>
</tr>
<tr>
<td>RAM</td>
<td>1GB</td>
</tr>
<tr>
<td>HDD</td>
<td><strong>외장</strong>-1TB</td>
</tr>
</tbody>
</table><p>ARM 아키텍처 환경은 많은 단점이 있었습니다.<br>
지원하지 않는것이 많았으며 높은 레벨의 루틴을 시도하기 어려웠습니다.</p>
<h3 id="서비스">서비스</h3>

<table>
<thead>
<tr>
<th>최종 가동 서비스</th>
<th>데몬</th>
</tr>
</thead>
<tbody>
<tr>
<td>WEB + CLOUD</td>
<td>APACHE + PYDIO</td>
</tr>
<tr>
<td>FTP</td>
<td>VSFTPD</td>
</tr>
<tr>
<td>DB</td>
<td>MYSQL</td>
</tr>
<tr>
<td>SHARE DIRECTORY</td>
<td>SAMBA</td>
</tr>
</tbody>
</table><p>웹 클라우드 또한 PYDIO와 OwnCloud 등 여러가지 소스를 시도했었습니다.</p>
<h2 id="v02-2020.11가동중">v02 (2020.11~가동중)</h2>
<h3 id="사진-1">사진</h3>
<p><img src="page/serverLog/02.png" alt="2 세대"></p>
<h3 id="제원-1">제원</h3>

<table>
<thead>
<tr>
<th>총 금액</th>
<th>~200,000원</th>
</tr>
</thead>
<tbody>
<tr>
<td>MB</td>
<td>ASROCK J5040-ITX</td>
</tr>
<tr>
<td>OS</td>
<td>Debian</td>
</tr>
<tr>
<td>CPU</td>
<td><strong>MB 결합</strong>-4Core 2~3.2GHz</td>
</tr>
<tr>
<td>RAM</td>
<td>8GB</td>
</tr>
<tr>
<td>HDD</td>
<td>2TB</td>
</tr>
<tr>
<td>SYSFAN</td>
<td>NOCTUA120mm</td>
</tr>
</tbody>
</table><p>오랫동안 계획된 시스템입니다.<br>
CPU가 결합된 상태에서 출고되는 저전력이지만 데스크탑 아키텍처인 보드를 구해 기반을 맞췄습니다.</p>
<h3 id="서비스-1">서비스</h3>
<p><strong>도커 컨테이너 내에서 작동되는 서비스는 cn_이라 표기</strong></p>

<table>
<thead>
<tr>
<th>서비스</th>
<th>상세</th>
</tr>
</thead>
<tbody>
<tr>
<td>cn_WEB1</td>
<td>APACHE_Main</td>
</tr>
<tr>
<td>cn_WEB2</td>
<td>HUGO</td>
</tr>
<tr>
<td>cn_WEB3</td>
<td>NextCloud</td>
</tr>
<tr>
<td>FTP</td>
<td>VSFTPD</td>
</tr>
<tr>
<td>cn_DB1</td>
<td>MARIADB</td>
</tr>
<tr>
<td>cn_DB2</td>
<td>MSSQL</td>
</tr>
</tbody>
</table><h3 id="관리-기록">관리 기록</h3>

<table>
<thead>
<tr>
<th>날짜</th>
<th>내용</th>
<th>사유</th>
</tr>
</thead>
<tbody>
<tr>
<td>2021.07.25</td>
<td>방화벽 추가 설정</td>
<td>불특정한 잦은 핑 수신</td>
</tr>
<tr>
<td>2021.08.06</td>
<td>SYSFAN 추가</td>
<td>발열문제로 인한 쓰로틀링, 여름이라 유난히 그런듯</td>
</tr>
<tr>
<td>2021.10.12</td>
<td>컨테이너 정리</td>
<td>체계적으로 분류하여 관리를 용이하게 함</td>
</tr>
</tbody>
</table>
