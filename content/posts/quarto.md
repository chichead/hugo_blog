---
title: '✏️ R Markdown의 차세대 포맷, Quarto'
date: '2022-08-27'
categories: ['R', 'Quarto']
showToc: true
ShowBreadCrumbs: true
comments : true
draft : false
---

## 🐣 Quarto가 뭐지

![](/images/quarto/quarto_thumbnail.jpeg)

RStudio는 자사의 2022년 컨퍼런스 rstudio::conf(2022)에서 발표한 여러 소식 가운데 가장 중요한 소식으로 이렇게 4가지를 꼽았다.

- **RStudio의 이름은 Posit으로 바꾼다**
- **새로운 오픈소스 기반의 과학기술 출판 시스템, Quarto**
- **Shiny 생태계의 새로운 발전**
- **tidymodel의 업데이트**

그 중에 Quarto가 이번 게시물의 주제이다. Quarto는 R Markdown에 이은 RStudio의 차세대 R 출판 플랫폼이다. 기존의 R Markdown을 이용하면 R code sript를 Word, HTML, PDF, PPT 등 다양한 문서 형식으로 만들 수 있었다. 웹을 통한 출판(Bookdown)까지도 가능했다.

![](/images/quarto/rmarkdown_env.png)

그런데 이 R Markdwon이 어느새 10년 가까이 지났다. 기능의 편리함은 지적할만한 게 없었지만 R Markdown 생태계가 너무 커져버렸다. 관련 생태계가 커졌다는 건 오히려 반길 일이지만 덕지덕지 붙어버린 서드파티 패키지들이 많아진 게 문제였다. 더 이상 통일된 하나의 R Markdown의 제작과 작업이 되질 못했다. 과학, 기술 블로그를 만들 땐 distill package를 사용하고, 웹 프레젠테이션 파일을 만들 땐 xaringan(사륜안) package를 사용하고…

그래서 등장한 게 바로 이 Quarto다. R Markdown과 마찬가지로 Knitr와 Pandoc을 기반으로 하고 있다. 궁극적으로 R Studio는 Quarto 생태계에 다른 언어를 사용하는 사람들끼리 모을 생각을 하고 있다. 그 이유 때문인지 Quarto는 R의 내장 라이브러리가 아닌 독립 소프트웨어로 제작되었다. 새로운 시스템 Quarto 단어가 생소할 텐데, Quarto는 4절판을 의미한다. 8페이지 분량의 텍스트를 두 번 접어서 네 장을 만드는 형식을 뜻한다. 출판 역사에 의미가 있는 단어를 골랐다고 한다.

Quarto는 [이 링크](https://quarto.org/docs/get-started/)에서 받을 수 있다. 링크를 들어가면 나오는 홈페이지에서도 확인할 수 있지만 Quarto는 R 뿐만 아니라 VS code, Jupyter에서도 활용할 수 있다.

 
## 🐣 Rmd와의 차이점

![](/images/quarto/rmarkdown_sys.png)

Quarto의 구조를 알기 위해선 R Markdown에 대한 이해가 필요하다. 일단 R Markdown 시스템은 위의 그림과 같다. Rmd(R 마크다운) 파일을 knitr package를 통해 md(마크다운) 파일로 만들고, pandoc 라이브러리를 통해 문서, PPT, 웹페이지, 책의 형태로 퍼블리싱되는 것. knitr은 2012년 Yihui Xie에 의해 개발된 패키지이다. Knitr 패키지를 이용하면 동적 리포트를 생성할 수 있게 해준다. md 파일을 다양한 형식으로 변환할 때에는 pandoc 라이브러리를 활용한다. 정리해보면 기존 R Markdown은 Rmd 파일을 여러 가지 형태의 문서로 퍼블리싱해주는 시스템이라고 할 수 있겠다.

![](/images/quarto/quarto.png)

Quarto도 비슷하다. R Markdown과 마찬가지로 Knitr과 pandoc을 활용한다. 달라진 건 적용 대상이다. 기존 시스템에선 Rmd만 가능했다면 이제는 Python도 가능하다. jupyter까지 활용하게 되면서 Python에서 qmd(Qarto markdown) 파일을 작성하면 jupyter를 통해 md 파일로 변환해 다양한 결과물을 만들어 낼 수 있게 된 것!

 
## 🐣 Quarto의 미래

![](/images/quarto/qaurto2.png)

RStudio의 이번 Qaurto 발표는 결국 Posit과 비슷하다. Python과 Julia 등 다른 언어들까지 포함하는 IDE인 Posit을 발표하고, 새롭게 출시한 Quarto에는 jupyter를 지원하면서 다른 언어 이용자들을 R 커뮤니티에 끌어들이겠다는 것. Python 이용자들도 충분히 웹사이트와 블로그, 책을 만들 수 있다고 유혹하는 것이다. RStudio의 CEO가 발표한 내용을 살펴보면 미래에는 마치 Google Docs에서 사람들이 자유롭게 문서를 편집하듯이 여러 언어를 사용하는 이용자들이 Quarto 문서를 통해 협업을 하길 구상하고 있다. 물론 아직까지 그런 환경이 갖춰져 있는 건 아니지만, 꽤나 매력적인 미래의 모습이다. 하루빨리 그런 환경이 오길 바라면서 이번 포스트를 마무리하겠다.












