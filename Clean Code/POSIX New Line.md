## POSIX New Line

> 왜 코드 파일은 비어있는 행으로 끝나야 하는가?

### Text File과 Line의 정의

IEEE가 제정한 UNIX의 API 규격인 **POSIX**(Portable Operating System Interface + X)는 이식성이 높은 UNIX 어플리케이션을 개발하기 위해 제정됨.

현재는 높은 이식성을 위해 여러 OS의 어플리케이션들이 POSIX의 규격을 따르고 있음. POSIX에서 정의하고 있는 `Text File`과 `Line`의 정의는 다음과 같음.

- **3.392 Text File**
  A file that contains characters organized into one or more lines. The lines do not contain NUL characters and none can exceed {LINE_MAX} bytes in length, including the <newline>. Although IEEE Std 1003.1-2001 does not distinguish between text files and binary files (see the ISO C standard), many utilities only produce predictable or meaningful output when operating on text files. The standard utilities that have such restrictions always specify "text files" in their STDIN or INPUT FILES sections.
- **3.205 Line**
  A sequence of zero or more non- <newline>s plus a terminating <newline>.

간단하게 해석하자면 `Text File`은 하나 이상의 `Line`으로 조직된 문자들을 포함하는 파일이며, `Line`은 **개행문자로 끝나는** 0개 이상의 개행문자가 아닌 시퀀스로 정의됨.

### EOL (End of Line)

즉, `Line`은 개행 문자로 끝나야 하며, `Text File`은 `Line`의 집합이므로, **`Text File` 역시 개행문자로 끝나야** 함. 개행문자로 끝나지 않은 `Line`은 `Line`으로 간주되지 않는데, 일부 프로그램에서는 **파일이 개행문자로 끝나지 않으면 마지막 줄을 처리하는데 문제가 발생**함. (개행문자가 없으므로 애초에 `Line`으로 간주되지 않음.)

반대로, `Line`이 개행문자로 종료되지 않으면 `cat`과 같은 유용한 명령어를 만드는 것이 훨씬 어려워짐. 파일의 끝이 개행문자로 끝나지 않으면 두 `Text File`의 첫 번째 줄의 시작과, 마지막 줄의 끝을 제대로 병합할 수 없음. (어느 부분을 기준으로 두 파일의 시작과 끝을 구분해서 병합할 것인가?)

최근에는 파일의 끝에 개행문자가 오지 않는다고 해서 큰 문제가 발생하진 않음. 그러나 Github에서는 파일의 끝에 개행문자가 없으면 `No newline at end of file`이라는 문구와 ⛔️ 모양을 파일 끝에 표시해 경고하며, 대부분의 IDE는 파일 끝에 자동으로 개행문자를 추가해주거나 추가할 수 있는 기능을 제공함.

### 결론

과거와 달리 최근에는 별 다른 문제를 일으키진 않으나, **파일의 끝에 개행문자를 추가하는 것은 좋은 습관**이며, 혹시 모를 호환성이나 다른 개발자와의 협업을 위해서라도 지킬 것을 추천.
