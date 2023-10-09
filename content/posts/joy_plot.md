---
title: '🍋 Joy Plot으로 그림 그리기'
date: '2023-10-06'
categories: ['데이터 시각화']
showToc: true
ShowBreadCrumbs: true
comments : true
draft : false
---

## 1. INTRO

9월 17일부터 20일까지 posit conf(2023)이 개최됐다. 해마다 posit(구 R studio)에서 진행하는 컨퍼런스인데 행사에 참여하진 못했지만 공개된 자료를 보는 맛이 있다. 올해도 깃헙에 공개된 워크샵 자료를 보다가 `Creative Coding in R`에 눈길이 갔다. 이 워크샵에선 데이터 시각화를 넘어서 자신만의 예술적 스타일을 만들어가는 데이터 아티스트의 사례와 작업물을 확인할 수 있었는데, 그중에서도 Pierre Casadebaig의 작업물이 눈길이 갔다.

Pierre Casadebaig는 crop ecology 분야의 과학자이면서, DEM 데이터를 활용한 작품을 만들고 있다. GIS를 다루었다면 어쩌면 한 번쯤은 접해봤을 DEM(Digital Elevation Model)은 지표면의 높이를 일정한 간격에 따라 측정해 만든 수치표고모형이다. 간단히 말하면 그리드(XY)에 고도 데이터(Z)가 들어있는 형태의 데이터 셋이다. 일종의 3차원 점 데이터라고 할 수 있겠다. 이 사람은 이 3차원 점 형태의 데이터를 2차원의 선으로 변환해 표현하고 있다. 

아래 이미지는 그의 블로그에서 올라와 있는 `ridge_04_heric.png`라는 작품이다. 파일 명에서 유추해 보건데 프랑스 heric 지방의 산등성이를 나타낸 그림일 것이다. 

![](/images/joyplot/ridge_04_heric.png)


## 2. CP 1919

Pierre Casadebaig는 천문학의 아름다운 시각화를 보고 영감을 받아 작품 활동을 하기 시작했다. 그가 영감을 받은 천문학의 아름다운 시각화는 바로 CP 1919다. 이름은 낯설 수 있지만 아마 그림은 많이 보았을 거다. 바로 이 시각화다.

![](/images/joyplot/cp_1919.png)

요 녀석은 인류 최초로 발견된 펄사(Pulsar) CP 1919의 스펙트럼을 시각화한 그래프다. 펄사는 전자기파를 뿜으며 자전하는 중성자별인데, 만약 이 전자기파가 지구를 향해 방출된다면 주기적으로 맥박이 뛰는 것 마냥 여러 파장이 주기적으로 관측된다. 처음 이 현상을 발견했을 땐 너무나도 부자연스럽게, 말 그대로 자연에서 발견되기 어려울 정도로 정확한 간격으로 파장이 방출되는 탓에 외계인이 보내는 메시지라고 생각했다. 그래서 이 파장에게 외계인을 뜻하는 Little Green Man의 이름을 따 LGM-1이라고 붙이기도 했다. 물론 시간이 지나 이 파장이 중성자별의 주기적 활동이라는 걸 알게 되었지만.

천문학적으로는 참으로 역사적이고 의미 있는 시각화이긴 하지만 그렇다고 많은 사람들이 알아줄리는 만무할 터. 이 그래프가 대중적으로 알려지게 된 건 JOY DIVISION의 영향이 컸다. JOY DIVISION은 1970-80년대 활동한 포스트 펑크 밴드다. 신스 팝 레전드 밴드인 New Order의 전신이기도 하다. 여튼 JOY DIVISION은 CP 1919의 스펙트럼 이미지를 1979년 발매한 정규 앨범 \<Unknown Pleasures\>의 커버로 사용했고, 이게 그 당시 힙스터들에게 엄청나게 유행하게 된 거다. 그 영향으로 이런 형태의 Ridge Plot의 다른 별칭이 JOY DIVISION의 이름을 딴 Joy Plot이다.

## 3. VIZ

CP1919를 보고 영감을 받은 Pierre Casadebaig은 프랑스 지역의 산을 대상으로 작품활동을 이어오고 있다. 아주 감사하게도 그는 자신의 블로그에 R로 어떻게 작업을 하고 있는지 개괄적으로 설명해두고 있다. 블로그 글을 참고 삼아 한 번 "우리나라를 가지고도 만들어 볼 수 있지 않을까?"해서 도전해보았다.

우리나라 DEM 데이터는 국가공간정보포털에서 받을 수 있다. 국가공간정보포털에서는 도 단위로 DEM 데이터를 제공해주고 있는데, 조금 더 자세한 정보는 국토지리정보원의 국토정보맵을 통해 받을 수 있다. 내가 사는 동네의 모습을 보기 위해 국토정보맵에서 자료를 다운받았다. 

```r
library(tidyverse)
library(terra)
library(raster)
library(stars)
library(elevatr)
```

이번 시각화에서 활용할 패키지들이다. `tidyverse`는 데이터 전처리, 시각화 등 전천후로 활용할 패키지 셋이고,`terra`와 `raster`는 r에서 공간정보를 분석할 때 활용할 수 있는 `rspatial` 패밀리에 있는 녀석들이다. 그 외에도 `stars`와 고도 데이터 분석을 위해 `elevatr` 패키지를 활용했다.

```r
# DEM 파일 raster / 고도 데이터 추출
dem_37705 <- terra::rast("/Users/admin/Downloads/37705.img") |>
  elevatr::get_elev_raster(z = 11, clip = "bbox")

 # xyz 데이터 셋 만들기
df_dem_37705 <- dem_37705 |>
  st_as_stars() |>
  as_tibble() |>
  select(x, y, z = 3)

df_dem_37705

# A tibble: 712,152 × 3
#         x        y     z
#     <dbl>    <dbl> <dbl>
# 1 955611. 1972288.    NA
# 2 955641. 1972288.    NA
# 3 955672. 1972288.    NA
# 4 955702. 1972288.    NA
# 5 955732. 1972288.    NA
# 6 955762. 1972288.   322
# 7 955792. 1972288.   321
# 8 955823. 1972288.   319
# 9 955853. 1972288.   315
#10 955883. 1972288.   308
# ℹ 712,142 more rows
# ℹ Use `print(n = ...)` to see more rows
```

국토정보맵에서 받은 파일은 2022 성동 37705 DEM 파일을 읽어온 뒤 그 중에서 고도 데이터를 추출했다. 추출한 데이터는 x, y, z 좌표료 표현할 수 있다. 앞서 이야기한 것 처럼 그리드(XY)에 고도 데이터(Z)가 들어있는 형태의 데이터 셋이니까. `scatterplot3D` 패키지를 이용해 간단히 시각화 하면 이렇게 표현할 수 있다. 71만 2,152개의 데이터를 다 그리기엔 뭐하니까 일부만 뽑아서 그려본다.

```r
library(scatterplot3d)

scatterplot3d(df_dem_37705[100:10000, ], color = "tomato", pch = 20)
```
![](/images/joyplot/dem_example.png)

pch(plot symbol character)가 20인 작은 점으로 3D 그래프를 그리면 이렇게 그릴 수 있다. 이번엔 모든 데이터를 가지고 y축 기준 라인을 그려보면 아래와 같이 그릴 수 있을 것이다. 이렇게 그려진 그래프는 서울의 모습을 정면에서 바라본 모습이라 y축 기준의 라인들이 좌라락 겹쳐져 있는 모습이다.

마치 이런 셈인거다. 서울 지도에서 위도를 기준으로 잘게 슬라이스를 하고 그 슬라이스한 카드를 겹쳐 놓는 식. 카드엔 고도 정보, 즉 산등성이의 모습이 담겨있다.

```r
ggplot(df_dem_37705) +
  geom_line(aes(x = x, y = z, group = y), linewidth = 0.1, alpha = 0.5) +
  theme_minimal()
```

![](/images/joyplot/dem_total.png)

오호라, 잘 나오고 있다. 

이제 우리가 할 것은 겹쳐 보이지 않도록 데이터를 조정해주는 것이다. y축을 기준으로 슬라이스를 해 두었으니 겹치지 않게 약간씩 시프트 해주는 과정을 거치도록 한다.

```r
z_shift = 5

data_index_37705 <- df_dem_37705 |>
  distinct(y) |>
  arrange(y) |>
  mutate(y_rank = rank(y),
         y_dist = scales::rescale(y, to = c(0, 1)),
         dz = y_rank * z_shift)

# 원래 데이터에다가 붙여주기
df_shift_37705 <- df_dem_37705 |>
  left_join(data_index_37705, by = "y") |>
  group_by(y) |>
  mutate(xn = x - min(x),
         x_rank = rank(x)) |> # x축을 원점으로 붙이는 과정
  ungroup() |>
  mutate(zs = z + dz) # z 시프트
```
y축을 기준으로 시프트를 할 것이기에 y_rank라는 칼럼을 만들어 그거에 따라 스케일을 조정했다. 그리곤 `z_shift` 값에 따라 새롭게 변경될 dz값을 넣어주었다. 위치가 미세조정된 데이터를 원래 데이터에 붙여주면 끝! `z_shift`값이 5일 때 시각화를 해보면 아래와 같이 나온다.

```r
ggplot(df_shift_37705) +
  geom_line(aes(x = x, y = zs, group = y), linewidth = 0.1) +
  theme_minimal()
```

![](/images/joyplot/zshift_5.png)

기존엔 정면에서 서울을 바라봤다면 이제는 위에서 내려다보는 형태로 바뀌었다.

`z_shift`값을 조정하면 바라보는 각도를 조정할 수 있다. 아래 그래프는 `z_shift`가 50일 때의 그래프이다. 시프트값이 5일때보다 더 위에서 바라보는 형국이다. `z_shift`값이 커질수록 더 위에서 바라보는 형태로 그래프를 그릴 수 있다.

![](/images/joyplot/zshift_50.png)

자, 일단은 y축으로 슬라이스해서 붙여놓긴 했다. 그런데 겹치는 선들이 문제다. 물론 겹치는 선으로 느껴지는 음영덕에 3D 입체 느낌이 나긴 하지만 제대로 된 표현은 아니니까, 겹치는 선을 없애는 작업을 해보자.

겹치는 선을 없애는 방법은? `z_threshold`값을 둔 다음에, 이 녀석보다 작은 거리에 있는 점은 없애버려서 겹치는 선들을 원천 차단해본다. 각 라인별로 겹치는 걸 확인하기 위해 x축을 기준으로 y축을 따라 이웃한 점들간의 거리를 계산하자. 모든 점에 대해서 계산할 필요는 없이 주변, 한 100개 정도만 계산해보자. 

```r
n_lag = 100
z_threshold = 6

distance_fun <- map(glue::glue("~ . - lag(., n = {1:n_lag})"), ~ as.formula(.))
distance_col <- glue::glue("zs_{1:n_lag}")

df_ridge_37705 <- df_shift_37705 |>
  arrange(y_rank) |>
  group_by(x) |>
  mutate(across(zs, .fns = distance_fun)) |>
  ungroup() |>
  mutate(zn = case_when(
    # distance_col에 모든 녀석들을 상대로 z_threshol보다 작으면 NA, 아니면 zs 그대로
    if_any(all_of(distance_col), ~ . < z_threshold) ~ NA_real_, 
    TRUE ~ zs)) |> 
  select(- all_of(distance_col))
```
주변 라인과의 거리를 계산하기 위해 lag 함수를 이용하겠다. 익명함수를 작성한 뒤 across 함수를 통해 일괄 적용해줬다. 이제 최종적으로 df_ridge_37705로 그림을 그리면 끝!

```r
ggplot(df_ridge_37705) +
  geom_line(aes(x = xn, y = zn, group = y), linewidth = 0.1) +
  theme_void()
```
![](/images/joyplot/final.png)

오른쪽 아래쪽에 오류로 보이는 삐죽 올라온 녀석이 눈에 거슬리긴 하지만 괜찮은 시각화 결과물을 건졌다.


## 📚 Reference

1. [posit::conf 2023 Creative Coding in R](https://github.com/posit-conf-2023/creative-coding)
2. [Pierre Casadebaig, blog](https://art.casadebaig.net/posts/works/dispyr/)