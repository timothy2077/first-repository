- [O]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 기능이 정상적으로 작동하는지?
        - 해당 조건을 만족하는 부분의 코드 및 결과물을 근거로 첨부
          
### 코드는 Pomodoro 타이머의 기능을 구현하고 있으며 작업 시간과 휴식 시간을 관리하는 기능이 정상적으로 잘 작동합니다. 

  // Pomodoro Timer를 시연하기 위한 메인 함수
void main() {
  PomodoroTimer timer = PomodoroTimer();

  // 타이머 시작
  timer.start();

  // 70초 후 멈춤 (실제 사용에서는 각 시간 주기를 더 길게 설정할 것)
//   Future.delayed(Duration(seconds: 70), () {
//     timer.stop();
//   });
}

- [O]  **2. 핵심적이거나 복잡하고 이해하기 어려운 부분에 작성된 설명을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation/markdown이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.
    
### 코드에 클래스와 메서드 이름이 명확하게 기능을 설명하고 있습니다. 

// TimerTask를 구현하고 TimerHelper를 사용하는 구체 클래스
class PomodoroTimer extends TimerTask with TimerHelper {
  static const int workTime = 25 * 60; // 25분을 초 단위로 변환
  static const int shortBreakTime = 5 * 60; // 5분을 초 단위로 변환
  static const int longBreakTime = 15 * 60; // 15분을 초 단위로 변환
  static const int repeatCount = 4; // 몇 cycle 돌릴건지?

        
- [O]  **3.** 에러가 난 부분을 디버깅하여 “문제를 해결한 기록”을 남겼나요? 또는
   “새로운 시도 및 추가 실험”을 해봤나요? ****
@@ -22,17 +43,82 @@
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.

### 코드에는 특별한 에러가 보이지 않으며 문제에서 요구하는 조건에 더해 추가적으로 수행한 시도들은 하기와 같습니다.

 @override
  void stop() {
    _timer?.cancel();
    _remainingTime = workTime;
    _cycleCount = 0;
    _isWorking = true;
    print('타이머가 멈췄습니다. 초기 상태로 리셋되었습니다.');
  }

  @override
  void tick() {
    if (_remainingTime > 0) {
      _remainingTime--;
      print('남은 시간: ${formatTime(_remainingTime)}');
    } 
    else {
      _cycleCount++;
      if (_isWorking) {
        if (_cycleCount % 4 == 0) {
          _remainingTime = longBreakTime;
          print(
              '작업 시간이 종료되었습니다. ${_cycleCount}회차 $longBreakTime분 휴식 시간을 시작합니다.');
        } 
        else {
          _remainingTime = shortBreakTime;
          print(
              '작업 시간이 종료되었습니다. ${_cycleCount}회차 $shortBreakTime분 휴식 시간을 시작합니다.');
        }
      } 
      else {
        // 반복횟수가 되면 동작하지 않게 함.
        if (_cycleCount < repeatCount) {
          _remainingTime = workTime;
          print('휴식 시간이 종료되었습니다. ${_cycleCount}회차 $workTime분 작업 시간을 시작합니다.');
        }
      }
      _isWorking = !_isWorking;

      // 4회차가 끝나면 타이머 멈춤
      if (_cycleCount == 4) {
        print('${_cycleCount}회차 사이클이 종료되었습니다. 타이머를 멈춥니다.');
        stop();

          
- [O]  **4. 회고를 잘 작성했나요?**
    - 프로젝트 결과물에 대해 배운점과 아쉬운점, 느낀점 등이 상세히 기록 되어 있나요?

### 작성자는 프로젝트를 통해 배운 점과 아쉬운 점, 느낀 점을 상세하게 구두로 표현하였고 이 파일 최하단에 솔직하게 기록하였습니다.

  /* ----- 회고 -----
 * 작성자 : 김소영
 * 노드 학습을 아직 끝내지 못한 상태에서 한 퀘스트라 걱정이 많이 됐지만 팀원분과 같이해서 수월하게 진행할 수 있었다.
 * 항상 python으로만 코딩하다 dart로 코딩하려니 문법적인 부분들에서 햇갈리는 부분들이 있었다.


- [O]  **5. 코드가 간결하고 효율적인가요?**
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 모듈화(함수화) 했는지
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.

### 코드는 간결하고 효율적으로 작성되었습니다. 특히, 추상 클래스와 믹스인을 사용하여 코드 중복을 최소화하고 있기애 모듈화가 잘 구현되었습니다.  

// 추상 클래스 -> 나중에 메서드를 정의하겠다.
abstract class TimerTask {
  void start();
  void stop();
  void tick();
}

// 믹스인 -> 기능 합성
mixin TimerHelper {
  String formatTime(int seconds) {
    final minutes = seconds ~/ 60;
    final remainingSeconds = seconds % 60;
    return '$minutes:${remainingSeconds.toString().padLeft(2, '0')}';
  }
}

