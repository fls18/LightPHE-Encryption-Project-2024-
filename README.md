# LightPHE-Encryption-Project-2024-
부분 동형암호(LightPHE)를 이용하여 암호화된 데이터를 복호화하지 않은 상태에서 비교해 보안성을 높인다. 사용 언어: python

생체정보의 특성상 유출되었을 때 치명적이다. 그렇기에 인증과정에서 암호화된 데이터를 복호화하지 않고 그대로 비교할 수 있는 동형암호는 효과적이다.
완전 동형암호를 이용하기에는 부피가 커, 이번 프로젝트에서는 부분 동형암호인 LightPHE과 지문인식 알고리즘을 결합하고, SAD 알고리즘으로 복호화없이 계산해 보안성이 뛰어난 지문인식 프로그램을 제작했다.







암호화 전 / 암호화 후 지문
<img width="1918" height="920" alt="image" src="https://github.com/user-attachments/assets/596de617-3dba-4dd6-a39f-94890211b17e" />

## 출처
- 지문 데이터 : https://github.com/kairess/fingerprint_recognition
- 지문 인식 알고리즘 : https://gogetem.tistory.com/entry/%EC%A7%80%EB%AC%B8-fingerprint-%EC%9D%BC%EC%B9%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-Python
- LightPHE 사용 방법 참고 : https://github.com/serengil/LightPHE
