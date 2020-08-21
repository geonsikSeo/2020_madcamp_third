# 2020_madcamp_third - 우연히 봄
Android project for week 3 of Madcamp@KAIST - 개발기간: 1주일
By 서건식, 황소진, 서규성

블루투스 기반 근거리 매칭 서비스 '우연히 봄'

출근길, 당신과 매일 같이 출근하는 사람들이지만 평생 모르고 살아갈 수도 있습니다.

평생 모르고 지나칠 수 있는 인연을 우연히 봄이 이어드립니다.


# Abstraction

### 서비스 설명

우연히 봄은 구글과 애플이 코로나 확진자 List를 추출하기 위해 블루투스 MAC ADDRESS를 받아와 근처에 있던

사람들에게 확진자 발생시 알림을 보내는 기술에서 아이디어를 얻어

매일 저녁 오후 7시, 출근길이나 지나가면서 스쳐간 인연들

일정시간 접촉시 Matching List에 추가해 소개해주는 서비스입니다.



### 친밀도 점수

접촉한 사람들 중에서도 어느 시간대에 접촉했는지, 얼마나 많이 접촉했는지, 사는 곳이 어디인지 등등

많은 요소를 결합해 알고리즘을 적용하여 친밀도 점수를 Backend에서 계산하여 보여줍니다.

이는 우연히 봄의 과금요소이며, 상단 메뉴에 소개된 인연들 중 가장 높은 친밀도 점수를 보여줍니다.

어떤 사람이 가장 높은 확률의 친밀도 점수를 가지고 있는지는 모르며, 이를 확인하기 위해선 결제가 필요합니다. (결제 시스템 미구현)



### 기타 기능

프로필 수정과 프로필 확인, 별점 보내기, 좋아요 보내기, 매칭 시 연락기능 등을 구현하였습니다.

Backend 코드는 해당 github에 있습니다. 

https://github.com/geonsikSeo/2020_madcamp_third-server


# Application Intro

![Loading](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/1.gif)

어플리케이션을 구동하면, 권한 허용 ViewPager와 어플 설명을 위한 IntroActivity를 실행한 후

Login Activity로 넘어갑니다.

# Sign Up

![Signup1](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/6.jpg){: width="100" height="100"}
![Signup2](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/7.jpg){: width="100" height="100"}
![Signup3](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/8.jpg){: width="100" height="100"}
![Signup4](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/9.jpg){: width="100" height="100"}

# Main Menu

![Menu](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/5.png)

메인메뉴는 해당 캡처 화면으로 구성되어 있습니다.

1. 자신의 프로필 사진을 클릭하면 자신의 프로필을 볼 수 있습니다.

2. 내 프로필을 클릭하면 프로필을 수정할 수 있습니다.

3. 우연히 봄을 클릭하면 오늘의 매칭리스트를 확인할 수 있습니다.

4. 받은 좋아요를 클릭하면 상대에게 받은 좋아요를 확인하고, 수락할 수 있습니다.

5. 매칭된 인연을 클릭시 수락한 상대의 전화번호를 확인할 수 있습니다.

6. 상점에서는 우연히 봄의 화폐 단위인 '송이'를 구매할 수 있습니다. (구현X)


# Matching List

![FoldingCell](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/2.gif)

블루투스 기반으로 검색된 인연들은, 매일 오후 7시에 새로 업데이트 됩니다.

프로필 구성은 Animation을 적용했고, Folding cell로 Onclick 시에 펼쳐지고 닫혀지도록 구현하였습니다.


### Bluetooth Foreground

블루투스 검색은 매칭 리스트가 시작되는 시점에 허용 여부를 물어보고,

이후 Foreground로 실행되며 계속해서 상대를 탐색합니다.

이때 주변 블루투스 MAC ADDRESS와 서버에 등록된 ID와 비교하여 주변에 우연히 봄 유저가 있는지를 파악하여

있을 시 Matching List에 추가합니다.

일정 시간이 지나면 종료되며, 이는 배터리 소모를 줄이기 위해 Timer를 설정해 둔 것입니다.

이에 대한 코드는 BluetoothService에 구현되어 있습니다.


### Fold

Fold 상태에서는 상대의 닉네임과 사진, 언제 만난 인연인지를 표시해줍니다.

낮 / 저녁 / 밤 으로 구성되어 있으며, List에 추가된 시점을 서버에서 받아와 Front에서 처리하여 구분해줍니다.

### Unfold

Unfold 상태에서는 상대 이름과 별점 만난 날짜와 만난 시각을 표시해 줍니다.

또한 List에 추가되었을 때와 제거되었을 때 각 지점을 gps로 받아와

언제부터 어디까지 만났던 인연인지를 표시합니다.

또한 프로필 버튼을 누르면, 상대의 프로필을 확인할 수 있습니다.

# Profile Info

![Profile](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/3.gif)


나이, 자기소개, 키, 학교, 전공, 직업, 취미, 흡연/음주 여부 그리고 사진을 보고 마음에 드는 인연이라면

좋아요와 높은 별점을 보내보세요.

서버와의 통신은 Retrofit 라이브러리를 사용해 request를 보내고 response를 받습니다.

해당 코드는 RetrofitService에 구현되어 있습니다.

    /* Retrofit */
    implementation 'com.google.code.gson:gson:2.8.5'
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'


# 자신의 프로필 수정

![MyProfile](https://github.com/geonsikSeo/2020_madcamp_third/blob/master/imageformd/4.gif)

메뉴의 내 프로필을 클릭하면 자신이 회원가입시 작성했던 소개 문구들과 사진등을 변경할 수 있습니다.

초기에 설정한 것 중 닉네임, 키, 생년월일 등은 수정이 불가능합니다.

