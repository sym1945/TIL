# Assertions
|Characters|Meaning|
|--|--|
|x(?=y)|**Lookahead assertion**: x 뒤에 y가 있는 x 선택|
|x(?!y)|**Negative lookahead assertion**: x 뒤에 y가 없는 x 선택|
|(?<=y)x|**Lookbehind assertion**: x 앞에 y가 있는 x 선택|
|(?<!y)x|**Negative lookbehind assertion**: x 앞에 y가 없는 x 선택|

# Boundaries
|Characters|Meaning |
|--|--|
|^|문자열의 시작|
|$|문자열의 끝|
|\b|*단어의 경계 (단어 = \w = [a-zA-Z0-9_])|
|\B|*단어의 경계 외|

***단어의 경계**
|hello|! |how| |are| |you|?

***단어의 경계 외**
h|e|l|l|o! h|o|w a|r|e y|o|u?

# Character Classes
|Characters|Meaning |
|--|--|
|.|임의의 문자 1개|
|\d|숫자 = [0-9]|
|\D|숫자가 아닌 것 = [^0-9]|
|\w|단어 = [a-zA-Z0-9_]|
|\W|단어가 아닌 것 = [^a-zA-Z0-9_]|
|\s|공백문자|
|\S|공백문자가 아닌 것|
|\t|탭 문자 (U+0009)|
|\r|캐리지 리턴 (U+000D)|
|\n|라인 피드 (U+000A)|
|\v|수직 탭 문자 (U+000B)|
|\f|폼 피드 (U+000C)|
|[\b]|백스페이스 (U+0008)|
|\0|널 문자 (U+0000)|
|\cX|문자열 내부 제어 문자 (X=[A-Z])|
|\xhh|코드가 hh인 2개의 16진수 (hex code) |
|\uhhhh|코드가 hhhh인 4개의 16진수 (uni code)|
|\\ |이스케이프|

# Groups and Ranges
|Characters|Meaning |
|--|--|
|x\|y|x 또는 y|
|[xyz]|문자셋. 괄호 안에는 어떤 문자도 가능(이스케이프 
|[^xyz]|부정 문자셋. 괄호 안에 없는 문자에 대응|
|(x)|**Capturing group**: x에 대응하고 이를 기억함|
|(?:x)|**Non-capturing group**: x에 대응하고 이를 기억하지 않음|
|(?\<a\>x)|**Named capturing group**: x에 대응하고 이를 a라는 그룹에 기억함
