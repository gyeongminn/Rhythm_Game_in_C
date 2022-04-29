
!youtube[flk778sG75g]

# 소개
> 1학년 텀프로젝트 과제로 만들었던 리듬게임입니다.

리듬게임 자체를 만드는건 오래 걸리지 않았던 것 같은데, 싱크를 맞춘다고 정말 고생했던 기억이 납니다.

---

# 사용법

D, F, J, K 키를 사용하여 노트를 입력합니다.

정확하게 노트를 입력하면 Excellent, 조금 느리게 입력하면 Good 판정이 됩니다.

연속으로 노트를 입력하게 되면 콤보가 쌓입니다.

콤보와 판정을 통해 점수가 쌓입니다. 점수 계산 식은 다음과 같습니다.
```c
score += (combo / 10 + 1) * 50
```
설정 메뉴에서 두가지 옵션을 켜고 끌 수 있습니다.

- '노트 찍기 모드'를 켜면 에디터 모드가 활성화 됩니다.
노래는 나오지만 노트는 내려오지 않습니다. 이 때, 노래에 맞춰 키보드를 입력하면 노트가 'note.txt' 파일에 저장되게 됩니다.

- '디버깅 모드'를 켜면 디버깅 모드가 활성화 됩니다.
화면이 새로고침 되는 딜레이를 확인할 수 있습니다.

---

# 설명
- 시작 부분의 간단한 텍스트 애니메이션은 for문을 통해 구현하였습니다.
이차원 char 배열로 텍스트를 저장해둔 뒤, 한 줄씩 출력/지우기를 반복하는 방식입니다.

- 노래를 선택하게 되면 노트가 저장된 텍스트 파일에서 char 배열로 버퍼를 받습니다. 
0이면 노트가 없는 것이고, 1이면 노트가 있는 것으로 판단합니다.

- 노래는 `PlaySound` 함수를 사용하여 재생하였습니다.

- 노트는 텍스트 색상을 바꾼 뒤, 공백을 출력하여 리듬게임의 노트인 것처럼 구현하였습니다. 
`SetConsoleTextAttribute` 함수를 통해 텍스트의 색상을 바꾸어 주었습니다.

- 한 화면의 딜레이는 노래의 BPM에 맞춰주었습니다.
노트가 빠르게 출력되어야 하기에 필연적으로 딜레이에 편차가 생기게 됩니다.
반복문의 시작과 끝에 시간을 측정하여, 딜레이를 균일하게 보정해 주었습니다.

- 더블 버퍼링을 응용하여 프론트 버퍼와 백 버퍼가 바뀌는 부분만 공백으로 지운 후, 출력하는 방식으로 구현하였습니다.

- 키 입력을 받는 것은 `GetAsyncKeyState` 함수를 사용하였습니다.
멀티 쓰레드를 위해 `_beginthreadex` 함수를 사용하였습니다. 여러 키를 동시에 입력받을 수 있게 해 줍니다.

- 판정선에 노트가 들어왔을 때, 화면 버퍼와 키 입력 상태를 비교하여 노트 입력 판정을 구분합니다.
프론트 버퍼와 백 버퍼의 상태를 비교하여 롱 노트와 숏 노트를 구분합니다.
노트를 정확하게 눌렀다면 Excellent, 빠르게 눌렀다면 Good, 놓쳤다면 Miss로 구분합니다.
`key_state` 변수를 만들어 키를 꾹 눌러 점수를 얻는 것을 방지하였습니다.

- 게임 중 Excellent, Good, Miss 판정을 기록합니다.
기록된 판정의 비율에 따라 랭크가 부여됩니다.
게임이 끝나게 되면 판정들을 보여주고, 비율에 따라 그래프를 그려 성취도를 직관적으로 보여줍니다.

# 콘솔 속성을 변경하고 플레이 하는것을 권장드립니다.

- 옵션 - 레거시 콘솔 사용 체크, 

- 글꼴 - 크기 10 x 22,
- 글꼴 - 글꼴 래스터 글꼴,

- 레이아웃 - 화면 버퍼 크기
- 너비 : 100, 높이 : 40
- 레이아웃 - 창 크기
- 너비 : 100, 높이 : 40
