# 캐릭터 8종 이미지 생성 프롬프트

2×2×2 구조 — **얼굴 길이**(짧은/긴) × **하관 폭**(각진/갸름) × **이목구비 간격**(넓은/좁은)

세 축의 조합으로 임베딩 공간에서 서로 최대한 멀어지게 설계했다. 아무 얼굴이나 8개 만들면
서로 가까워져서 매칭이 동전 던지기가 된다.

| # | 얼굴 길이 | 하관 | 이목구비 | 안경 | 그룹 |
|---|---|---|---|---|---|
| 01 | 짧은 | 각진 | 넓은 간격 | — | A |
| 02 | 짧은 | 각진 | 좁은 간격 | ○ | A |
| 03 | 짧은 | 갸름 | 넓은 간격 | — | B |
| 04 | 짧은 | 갸름 | 좁은 간격 | — | B |
| 05 | 긴 | 각진 | 넓은 간격 | — | A |
| 06 | 긴 | 각진 | 좁은 간격 | — | A |
| 07 | 긴 | 갸름 | 넓은 간격 | ○ | B |
| 08 | 긴 | 갸름 | 좁은 간격 | — | B |

---

## ⚠️ 먼저 읽을 것

**1. 스타일 블록을 반드시 8장 모두에 똑같이 붙일 것.**
조명·배경·화각이 캐릭터마다 다르면, 임베딩이 "얼굴 차이"가 아니라 "조명 차이"를 학습한 것처럼
동작해서 매칭이 완전히 망가진다. 얼굴 묘사만 바꾸고 나머지는 글자 하나 건드리지 말 것.

**2. 반드시 한국인 얼굴로 생성할 것.**
관람객이 한국인이므로 캐릭터도 같은 분포 안에 있어야 한다. 서양인 얼굴로 만들면 모든 관람객이
8종 전부에서 똑같이 멀어져서 유사도가 의미를 잃는다.

**3. 프로토타입용 변형은 image-to-image로 만들 것.**
텍스트 프롬프트만으로 다시 뽑으면 다른 사람이 나온다. 캐릭터 레퍼런스 기능이나 i2i로
"같은 사람"을 유지해야 평균 벡터가 뭉개지지 않는다.

---

## 공통 스타일 블록 (8장 전부에 동일하게)

```
Photorealistic corporate ID badge portrait of a Korean adult in their mid-20s.
Studio headshot, straight-on frontal view, camera exactly at eye level, head and
shoulders only, head centered and filling about 70% of the frame height.
Seamless light grey (#F0F0F2) studio backdrop, evenly lit with no visible shadow
on the background. Soft frontal key light from a large softbox with subtle fill,
no harsh shadows on the face, neutral white balance around 5500K. Neutral
expression with a very subtle closed-lip smile, eyes looking directly into the
lens. Plain dark navy collared shirt, only the collar visible at the bottom edge
of the frame. Evenly sharp across the entire face, shot on an 85mm lens at f/5.6,
no lens distortion. 3:4 vertical aspect ratio, high resolution, natural skin
texture with visible pores, no beauty retouching.
```

## 공통 네거티브 프롬프트

```
wide-angle distortion, fisheye, tilted head, three-quarter view, profile view,
dramatic side lighting, colored gel lighting, hard shadows, busy background,
dark background, hat, heavy makeup, large earrings, necklace, hands, full body,
multiple people, text, watermark, logo, oversaturated, plastic airbrushed skin
```

---

## 캐릭터별 얼굴 묘사

각 항목을 **공통 스타일 블록 앞에** 붙여서 사용한다.

### 01 — 짧은 얼굴 · 각진 하관 · 넓은 간격

```
A man with a short compact face and a broad square jawline creating a wide lower
face. Widely spaced eyes with a broad flat nose bridge, short nose, straight
thick eyebrows. Short cropped black hair with the forehead fully exposed.
Clean-shaven.
```

### 02 — 짧은 얼굴 · 각진 하관 · 좁은 간격 · 안경

```
A man with a short face and a strong square jaw. Closely set eyes with a narrow
nose bridge, single-eyelid eyes, compact central features clustered toward the
middle of the face. Short black hair with a neat side part. Wearing thin
rectangular black-framed glasses. Clean-shaven.
```

### 03 — 짧은 얼굴 · 갸름한 하관 · 넓은 간격

```
A woman with a short rounded face narrowing to a soft small chin. Widely spaced
large round double-eyelid eyes, small rounded nose, gently curved eyebrows.
Chin-length blunt black bob tucked behind the ears.
```

### 04 — 짧은 얼굴 · 갸름한 하관 · 좁은 간격

```
A woman with a short face and a delicate pointed chin. Closely set almond-shaped
eyes, short philtrum, small mouth, compact central features. Shoulder-length
black hair with soft waves and a center part.
```

### 05 — 긴 얼굴 · 각진 하관 · 넓은 간격

```
A man with a long face, high flat cheekbones and a wide angular jaw. Widely
spaced eyes, long straight nose, high broad forehead. Short black hair swept
back away from the face. Clean-shaven.
```

### 06 — 긴 얼굴 · 각진 하관 · 좁은 간격

```
A man with a long narrow face and a defined square chin. Closely set deep-set
eyes, long narrow nose bridge, thin lips, features gathered toward the center.
Medium-length black hair parted to one side.
```

### 07 — 긴 얼굴 · 갸름한 하관 · 넓은 간격 · 안경

```
A woman with a long oval face tapering to a narrow chin. Widely spaced
double-eyelid eyes, slim straight nose, long neck. Long straight black hair
falling past the shoulders. Wearing thin round metal-framed glasses.
```

### 08 — 긴 얼굴 · 갸름한 하관 · 좁은 간격

```
A woman with a long slender face and a sharp V-shaped jawline. Closely set
upward-slanting eyes, high cheekbones, narrow nose, compact central features.
Black hair pulled back into a low ponytail.
```

---

## 프로토타입용 변형 (캐릭터당 8~10장)

표시용 1장을 만든 뒤, 그 이미지를 레퍼런스로 삼아 i2i로 변형을 뽑는다.

```
Same person, same studio lighting, same background, same framing.
Vary only: head rotation up to 10 degrees left or right, chin raised or lowered
by up to 5 degrees, expression ranging from fully neutral to a slight closed-lip
smile, and a very slight shift in key light angle.
```

**주의** — 변형이 과하면 다른 사람이 되어 평균 벡터가 흐려지고, 8종이 서로 비슷해진다.
반대로 완전히 똑같은 이미지만 쓰면 프로토타입이 과하게 뾰족해져서 매칭이 안 된다.
"같은 사람의 다른 사진" 정도가 맞다.

총 생성량은 8종 × (1 + 9) ≈ **80장**.

---

## 만든 뒤 검증

1. **프로토타입 간 유사도 행렬** — 8×8 코사인 유사도를 전부 계산한다. 특정 쌍이 유독 높으면
   그 둘은 시스템이 구분하지 못한다는 뜻이므로 얼굴형을 더 벌려 재생성한다.
2. **매칭 분포 테스트** — 사람 얼굴로 돌려 8종에 골고루 퍼지는지 본다.
   한 캐릭터에 쏠리면 그 캐릭터가 평균 얼굴에 너무 가깝다.
   같은 테스트에서 교차 매칭률을 재고, 그 수치로 소프트 게이트 가중치 α를 정한다.

> 8종은 4종보다 셀당 검증 표본이 절반으로 줄어든다. 분포 테스트 인원을 **최소 40명**은
> 확보해야 캐릭터당 5명이 되어 해석이 가능하다.

---

## 인쇄용 버전 (감열 프린터)

감열식은 **흑백 1비트**라 실사 사진을 그대로 보내면 뭉개진다. 8장 각각에 대해
인쇄 전용 버전을 미리 만들어 둔다.

```python
from PIL import Image, ImageEnhance

img = Image.open("char_01.png").convert("L")        # 그레이스케일
img = ImageEnhance.Contrast(img).enhance(1.8)        # 대비 강화
img = img.resize((360, 480))                         # 인쇄 크기
img = img.convert("1")                               # Floyd-Steinberg 디더링
img.save("char_01_print.png")
```

대비 계수는 실제 프린터로 뽑아보며 조정한다. 화면에서 예뻐 보이는 값과
감열지에서 잘 나오는 값이 다르다.
