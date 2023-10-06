---
title: '🍋 Joy Plot으로 그림 그리기'
date: '2023-10-06'
categories: ['데이터 시각화']
showToc: true
ShowBreadCrumbs: true
comments : true
draft : true
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

CP1919를 보고 영감을 받은 Pierre Casadebaig은 프랑스 지역의 산을 대상으로 작품활동을 이어오고 있다. 아주 감사하게도 그는 자신의 블로그에 R로 어떻게 작업을 하고 있는지 개괄적으로 설명해두고 있다. 블로그 글을 참고 삼아 한 번 "우리나라를 가지고도 만들어 볼 수 있지 않을까?"해서 도전해보려 한다.


```r

library(tidyverse)
library(stars)
library(dplyr)


#------------------------------------------------------------------------------

polygon_kor <- rnaturalearth::ne_countries(
  country = c("South Korea"), scale = "medium", returnclass = "sf") |>
  st_transform(4166)

coord_kor <- st_sample(polygon_kor, size = 1) |>
  st_as_sf()

dem_kor <- elevatr::get_elev_raster(
  coord_kor, z = 11, clip = "bbox")

ggplot() +
  geom_stars(data = st_as_stars(dem_kor)) +
  geom_sf(data = coord_kor, color = "white", size = 1) +
  scale_fill_viridis_c(name = "elevation") +
  labs(x = "lon", y = "lat")



z_shift = 5

df_dem_kor <- dem_kor |>
  stars::st_as_stars() |>
  as_tibble() |>
  select(x, y, z = 3)

data_index_kor <- df_dem_kor |>
  distinct(y) |>
  arrange(y) |>
  mutate(y_rank = rank(y),
         y_dist = scales::rescale(y, to = c(0, 1)),
         dz = y_rank * z_shift)

df_shift_kor <- df_dem_kor |>
  left_join(data_index_kor, by = "y") |>
  group_by(y) |>
  mutate(xn = x -min(x),
         x_rank = rank(x)) |>
  ungroup() |>
  mutate(zs = z + dz)

ggplot(df_dem_kor) +
  geom_line(aes(x = x, y = z, group = y)) +
  theme_minimal()

ggplot(df_shift_kor) +
  geom_line(aes(x = x, y = zs, group = y)) +
  theme_minimal()


n_lag = 100
z_threshold = 5

list_distance <- map(glue::glue("~ . - lag(., n = {1:n_lag})"), ~ as.formula(.))
list_col <- glue::glue("zs_{1:n_lag}")


df_ridge_kor <- df_shift_kor |>
  arrange(y_rank) |>
  group_by(x) |>
  mutate(across(zs, .fns = list_distance)) |>
  ungroup() |>
  mutate(
    zn = case_when(
      if_any(all_of(list_col), ~ . < z_threshold) ~ NA_real_, TRUE ~ zs)
  ) |>
  select(- all_of(list_col))

ggplot(df_ridge_kor) +
  geom_line(aes(x = xn, y = zn, group = y)) +
  theme_minimal()



#---------------------

library(terra)
library(raster)

# DEM 파일 raster / 고도 데이터 추출
dem_37705 <- terra::rast("/Users/admin/Downloads/37705.img") |>
  elevatr::get_elev_raster(z = 11, clip = "bbox")

# xyz 데이터 셋 만들기
df_dem_37705 <- dem_37705 |>
  st_as_stars() |>
  as_tibble() |>
  select(x, y, z = 3)

# 시각화
install.packages("scatterplot3d")
library(scatterplot3d)

scatterplot3d(df_dem_37705[100:10000, ], color = "tomato", pch = 16)
scatterplot3d(df_shift_37705[100:10000, c(1, 6, 3)], color = "tomato", pch = 16)

df_shift_37705[, c(1, 6, 3)]

ggplot() +
  geom_stars(data = st_as_stars(dem_37705)) +
  scale_fill_viridis_c(name = "고도") +
  theme_minimal()

ggplot(df_dem_37705) +
  geom_line(aes(x = x, y = z, group = y), linewidth = 0.1, alpha = 0.5) +
  theme_minimal()

# 데이터 조정
# 여기서의 조정 목표는 y축, 즉 위도에 따라 슬라이스를 하고
# 겹쳐 보이지 않도록 약간씩 시프트해주자

z_shift = 5

data_index_37705 <- df_dem_37705 |>
  distinct(y) |>
  arrange(y) |>
  mutate(y_rank = rank(y),
         y_dist = scales::rescale(y, to = c(0, 1)),
         dz = y_rank * z_shift)

# 그리고 다시 원래 데이터에다가 붙여주지

df_shift_37705 <- df_dem_37705 |>
  left_join(data_index_37705, by = "y") |>
  group_by(y) |>
  mutate(xn = x - min(x),
         x_rank = rank(x)) |> # x 축 원점으로 붙이는 과정
  ungroup() |>
  mutate(zs = z + dz) # z 시프트


# 그러면 기존엔 정면에서 지역을 바라보는 형식이
# 시프트 이후엔 위에서 내려다보는 모습으로 변경
# z_shift 값에 따라 바라보는 각도를 조정할 수 있음

ggplot(df_shift_37705) +
  geom_line(aes(x = x, y = zs, group = y)) + # 시프트된 y(zs)로 시각화
  theme_minimal()

# shift가 더 되는 경우

scatterplot3d(df_dem_37705[100:10000, ], color = "tomato", pch = 16)
scatterplot3d(df_shift_37705[100:10000, c(1, 6, 3)], color = "tomato", pch = 16) # shift = 5
scatterplot3d(df_shift_37705_2nd[100:10000, c(1, 6, 3)], color = "tomato", pch = 16) # shift = 50


# 참고.
z_shift = 50

data_index_37705_2nd <- df_dem_37705 |>
  distinct(y) |>
  arrange(y) |>
  mutate(y_rank = rank(y),
         y_dist = scales::rescale(y, to = c(0, 1)),
         dz = y_rank * z_shift)

df_shift_37705_2nd <- df_dem_37705 |>
  left_join(data_index_37705_2nd, by = "y") |>
  group_by(y) |>
  mutate(xn = x - min(x),
         x_rank = rank(x)) |> # x 축 원점으로 붙이는 과정
  ungroup() |>
  mutate(zs = z + dz) # z 시프트


ggplot(df_dem_37705) +
  geom_line(aes(x = x, y = z, group = y), linewidth = 0.1) +
  theme_minimal()

ggplot(df_shift_37705) +
  geom_line(aes(x = x, y = zs, group = y), linewidth = 0.1) +
  theme_minimal()

ggplot(df_shift_37705_2nd) +
  geom_line(aes(x = x, y = zs, group = y), linewidth = 0.1) +
  theme_minimal()


# 그려보면 일단 y축으로 슬라이스해서 붙여놓긴 했다. 그런데 겹치는 선들이 문제
# 이제 해야할 건 겹치는 선 죽이기
# z_threshold보다 작은 거리에 있는 점은 없애버려서 겹치는 선들을 원천 차단하자

# 슬라이스한 라인에서 겹치는 걸 확인하기 위해 x를 기준으로 y를 따라 이웃점들간의 거리를 계산한다
# 모든 점에 대해서 계산할 필요는 없으니까 개수는 100개 정도로 제한하자
# 거리가 threshold 보다 낮으면 제거
# threshold를 조정하면서 이미지를 변화시킬 수 있음

# 예를 들어보자

test <- tribble(
  ~A, ~B, ~C,
  1, 1, 10,
  2, 1, 11,
  3, 1, 12,
  4, 1, 14,
  5, 1, 15,
  6, 1, 15,
  7, 1, 14, 
  8, 1, 12,
  9, 1, 11,
  10, 1, 10,
  1, 2, 13,
  2, 2, 13.3,
  3, 2, 13.5,
  4, 2, 14,
  5, 2, 14.5,
  6, 2, 14.5,
  7, 2, 14, 
  8, 2, 13.5,
  9, 2, 13.3,
  10, 2, 13,
  1, 3, 11.5,
  2, 3, 11.5,
  3, 3, 11.5,
  4, 3, 11.5,
  5, 3, 11.5,
  6, 3, 11.5,
  7, 3, 11.5, 
  8, 3, 11.5,
  9, 3, 11.5,
  10, 3, 11.5
)

ggplot(test) +
  geom_line(aes(x = A, y = C, group = B, color = as.factor(B)))


test |>
  arrange(B) |>
  group_by(A) |>
  mutate(new = across(C, ~ lag(., n = 1)), # 첫번째 라인과의 차이를 보기 위해
         new2 = across(C, ~ lag(., n = 2))) |> # 두번째 라인과의 차이를 보기 위해
  View()

# 거리를 계산하기 위해선 lag 함수를 통해 얻은 다른 라인과의 차이를 계산하면 됨
# 그 함수를 익명함수로 표현하면 ~. - lag(., n = 1) / ~. - lag(., n = 2) / ... / n = 100까지

n_lag = 100
z_threshold = 5


distance_fun <- map(glue::glue("~ . - lag(., n = {1:n_lag})"), ~ as.formula(.))
distance_col <- glue::glue("zs_{1:n_lag}")

df_ridge_37705 <- df_shift_37705 |>
  arrange(y_rank) |>
  group_by(x) |>
  mutate(across(zs, .fns = distance_fun)) |>
  ungroup() |>
  mutate(zn = case_when(
    if_any(all_of(distance_col), ~ . < z_threshold) ~ NA_real_, # distance_col에 모든 녀석들을 상대로 z_threshol보다 작으면 NA
    TRUE ~ zs)) |> # 아니면 zs 그대로
  select(- all_of(distance_col))


ggplot(df_ridge_37705) +
  geom_line(aes(x = xn, y = zn, group = y), linewidth = 0.1) +
  theme_minimal()
```


국가공간정보포털에서는 도 단위로 DEM 데이터를 제공하고 있음
국토지리정보원에선 국토정보맵을 통해 지역별 공간정보제공
내가 살던 답십리의 모습을 보기 위해 ‘2022 성동 37705’ DEM 파일을 받음

시각화를 위해 활용한 패키지들
tidyverse ; 데이터 전처리, 시각화 등 전천후로 활용할 패키지 셋
terra; 공간 데이터 분석을 위한 패키지 (stars도 사용)
elevatr; 고도 데이터 분석을 위해 사용할 패키지



