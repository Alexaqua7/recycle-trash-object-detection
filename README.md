# 🧠 재활용 품목 분류를 위한 Object Detection 

<div align="center">
  <img src="https://img.shields.io/badge/Python-3.8+-blue?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/MMDetection-3.x-green?style=for-the-badge&logo=github">
  <img src="https://img.shields.io/badge/Detection-Recycling-blueviolet?style=for-the-badge">
</div>

---

## 🍃 프로젝트 개요

현대 사회는 대량 생산·소비로 인한 **쓰레기 처리 문제**에 직면하고 있습니다.  
잘못 분리배출된 쓰레기는 재활용되지 못하고 환경오염의 주범이 됩니다.

본 프로젝트는 **이미지 기반 쓰레기 객체 탐지 모델**을 개발하여,  
보다 정확하고 효율적인 **분리수거 자동화** 및 **환경 교육 도구**를 운용하는 것을 목표로 합니다. 🌍

---

## 👥 팀원 소개

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/b17ce868-5498-4acf-8831-31829f8f7cbd" width="150px;" alt="서승환"/><br />
      <b>서승환</b><br />
      프로젝트 총괄, <br />Cascade R-CNN & ATSS 실험, <br />Cross Validation 구성
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/7c44b0c5-927a-4c65-8d21-8e240bcf1618" width="150px;" alt="강대민"/><br />
      <b>강대민</b><br />
      Streamlit 앱 개발, <br />MMDetection3 구성, <br />YOLOv8/11, DETR 실험
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/fc431d0d-51d5-4774-b900-67bc6a2bb2b5" width="150px;" alt="김홍주"/><br />
      <b>김홍주</b><br />
      Co-DETR, CenterNet2 실험, <br />앙상블 구성 및 튜닝
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/ddebfbe1-317d-4bf7-915c-524e51e5bd69" width="150px;" alt="박나영"/><br />
      <b>박나영</b><br />
      EfficientDet 실험, <br />TTA 적용
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d155ec79-8d03-45d4-b703-44a848b9b463" width="150px;" alt="이종서"/><br />
      <b>이종서</b><br />
      RetinaNet 실험, <br />W&B 연동
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/9a15231a-b69d-447f-9070-f58b29ccdcec" width="150px;" alt="이한성"/><br />
      <b>이한성</b><br />
      DINO, YOLOv3, FCOS 실험, <br />Baseline 코드 모듈화
    </td>
  </tr>
</table>

---

## 🗂 데이터 구성

- **Input**:
  - 쓰레기 객체가 포함된 이미지
  - COCO format의 bounding box annotation

- **Output**:
  - 객체의 `bbox 좌표`, `class`, `confidence score`
  - 예측 결과는 `.csv` 형태로 제출

---

## 🚀 최종 사용 모델

본 프로젝트에서는 다양한 최신 모델을 실험 후, 다음 모델들을 최종 채택하여 성능을 극대화했습니다:

- 📷 **Cascade R-CNN**
- ⚡ **YOLO v11**
- 🎯 **ATSS**
- 🧩 **Co-DETR**
- 🦖 **DINO**

---

## 🛠 설치 및 실행 방법

### 1️⃣ 환경 세팅

```bash
apt update && apt upgrade -y
apt install -y libgl1-mesa-glx libglib2.0-0 wget
pip install ninja
```

### 2️⃣ 레포지토리 클론 및 데이터 다운로드

```bash
git clone https://github.com/boostcampaitech7/level2-objectdetection-cv-04.git
cd level2-objectdetection-cv-04
```

```bash
cd data
wget https://aistages-api-public-prod.s3.amazonaws.com/app/Competitions/000325/data/data.tar.gz
tar -zxvf data.tar.gz
```

### 3️⃣ 라이브러리 설치

```bash
# mmcv-full 항목 제거 후 설치
pip install -r requirements.txt
```

---

## 🏋️‍♂️ 모델 학습 예시

```bash
python train.py \
  --traindata_dir ./data/train \
  --traindata_info_file ./data/train.csv \
  --save_result_path ./train_result \
  --log_dir ./logs \
  --val_split 0.2 \
  --transform_type albumentations \
  --batch_size 64 \
  --model_type timm \
  --model_name eva02_large_patch14_448.mim_m38m_ft_in22k_in1k \
  --pretrained True \
  --learning_rate 0.001 \
  --epochs_per_lr_decay 2 \
  --scheduler_gamma 0.1 \
  --epochs 5
```

> 💡 하이퍼파라미터는 자유롭게 조정 가능합니다.

---

## 📤 추론 실행

```bash
python inference.py
```

---

## 📁 프로젝트 구조

```
project_root/
├── dataset/
│   ├── train/, test/
│   ├── train.json, test.json
│
├── mmdetection/
│   ├── configs, tools, models ...
│   ├── train.py, inference.py
│
├── detectron2/
│   ├── train.py, inference.py
│
├── pytorch_detection/
│   ├── train.py, inference.py
│   └── src/
│       ├── config.py, model.py, trainer.py ...
│
├── requirements.txt
└── README.md
```

---

## 🧪 실험 가이드라인 (진행기간 ~2024.10.24)

### 실험 절차

1. `develop` 브랜치에서 새 브랜치 생성  
2. **Notion**에 실험 계획 업로드  
3. 브랜치명은 `exp/{실험내용}` 형식으로  
4. 세부 실험은 **Issue**로 정리  
5. 결과는 [📊 Google Sheet](https://docs.google.com/spreadsheets/d/1tuTotQ_ALJQyJPzXt2NMeeyWfkm5csweRrYfWxnff8A/edit?usp=sharing)에 기록  
6. 실험 완료 시 `develop` 브랜치로 Pull Request

---

## 📂 실험 폴더 규칙

- 폴더명: `#{Issue Number} {설명(optional)}`
- 필수 파일: `train.py`, `inference.py`
- Config 파일 변경 시 기존 경로 기준으로 분리

---

## 🌱 브랜치 네이밍 규칙

| 유형       | 네이밍 예시             |
|------------|--------------------------|
| 기능 구현  | `feat/augmentation`     |
| 버그 수정  | `fix/annotation-path`   |
| 실험용 브랜치 | `exp/yolov8-lr-sweep`  |

> 🔒 `main`, `develop` 브랜치는 직접 수정하지 말고 Pull Request를 이용해주세요.

📢 기여 및 수정 요청은 **Issue 또는 Pull Request**를 통해 자유롭게 해주세요!
