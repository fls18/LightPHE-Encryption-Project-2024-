# Paillier)-Encryption-Project-2024-


## 프로젝트 목적:
부분 동형암호(LightPHE)를 이용하여 암호화된 데이터를 복호화하지 않은 상태에서 비교해 보안성을 높입니다. 
사용 언어: python

## 설명:
생체정보의 특성상 유출되었을 때 치명적입니다. 그렇기에 인증과정에서 암호화된 데이터를 복호화하지 않고 그대로 비교할 수 있는 동형암호는 효과적이라고 할 수 있습니다.
완전 동형암호를 이용하기에는 부피가 커, 이번 프로젝트에서는 부분 동형암호인 LightPHE과 지문인식 알고리즘을 결합하고, SAD 알고리즘으로 복호화없이 계산해 보안성이 뛰어난 지문인식 프로그램을 제작했습니다.

## 코드 

```
    import cv2
    import numpy as np
    import os
    from lightphe import LightPHE

    cs = LightPHE(algorithm_name="Paillier", key_file="private.txt")
```
Paillier 알고리즘 사용

```    
    def preprocess_image(image_path):
        """이미지를 로드하고 전처리합니다."""
        print(f'Trying to load image: {image_path}')  # 경로 출력
        img = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)  # 흑백으로 로드
        img = cv2.resize(img, (90, 90))  # 크기를 90x90으로 조정
        img = img.astype(np.float32) / 255.0  # 정규화
        return img
    
    def flatten_image(image):
        """이미지를 1D 배열로 평탄화합니다."""
        return image.flatten()
```
전처리, 평탄화
    

```
    def encrypt_image(data, cs):
        """데이터를 암호화합니다."""
        encrypted_data = [cs.encrypt(int(v * 255)) for v in data]
        return encrypted_data
    
    def decrypt_image(encrypted_image, cs):
        decrypted_data = [cs.decrypt(v) / 255.0 for v in encrypted_image]
        return decrypted_data
```
암호화


```    
    def encrypted_sad(img1_enc, img2_enc, cs):
        """SAD: 픽셀 차이 합(SAD) 계산"""
        total_enc = cs.encrypt(0)
        for e1, e2 in zip(img1_enc, img2_enc):
            diff_enc = cs.add(e1, cs.neg(e2))  # e1 - e2
            abs_diff = cs.encrypt(abs(cs.decrypt(diff_enc)))  # SAD 단순화
            total_enc = cs.add(total_enc, abs_diff)
        return total_enc
```
SAD 계산하는 함수


```    
    def find_best_match(input_image_path, dataset_path):
        """입력된 이미지와 데이터셋의 이미지 간의 유사도를 계산하여 가장 높은 유사도를 가진 이미지를 찾습니다."""
        input_image = preprocess_image(input_image_path)
        input_flat = flatten_image(input_image)
        input_enc = encrypt_image(input_flat, cs)
    
        best_match = None
        best_similarity = float('inf')
    
        for filename in os.listdir(dataset_path):
            if filename.lower().endswith(('.jpg', '.png', '.bmp')): # 이미지 파일 형식
                dataset_image = preprocess_image(os.path.join(dataset_path, filename))
                dataset_flat = flatten_image(dataset_image)
                dataset_enc = encrypt_image(dataset_flat, cs)
    
                enc_sad = encrypted_sad(input_enc, dataset_enc, cs) 
                sad_score = cs.decrypt(enc_sad) # 비교 위해 복호화된 이미지 원래 형태로 변환(확인용)
    
                print(f"{filename}: SAD = {sad_score}")
    
                if sad_score < best_similarity:
                    best_similarity = sad_score
                    best_match = filename
    
        return best_match, best_similarity
    
    input_image_path = r'D:\data\input_fingerprint\sample_finger.jpg'  """ 입력 지문 이미지 경로"""
    dataset_path = r'D:\data1\dateset_fingerprint'  """ 지문 데이터셋 경로"""
```
유사도 확인


```    
    try:
        best_match, best_similarity = find_best_match(input_image_path, dataset_path)
        print(f'Best match: {best_match} with similarity: {best_similarity:.4f}')
    
    except ValueError as e:
        print(e)
```





## 암호화 전 / 암호화 후 지문
<img width="1918" height="920" alt="image" src="https://github.com/user-attachments/assets/596de617-3dba-4dd6-a39f-94890211b17e" />

## 출처
- 지문 데이터 : https://github.com/kairess/fingerprint_recognition
- 지문 인식 알고리즘 : https://gogetem.tistory.com/entry/%EC%A7%80%EB%AC%B8-fingerprint-%EC%9D%BC%EC%B9%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-Python
- LightPHE 사용 방법 참고 : https://github.com/serengil/LightPHE
