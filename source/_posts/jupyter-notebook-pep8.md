---
title: Jupyter Notebook 에 PEP8 적용하기
date: 2017-06-24 14:08:42
tags:
---

<!-- toc -->

Python 으로 데이터 분석을 할때 [Jupyter Notebook][1] 을 사용하면 공유나 문서화에 아주 큰 장점이 있습니다.

그런데 Jupyter Notebook 을 사용하여 여러사람들과 작업하다보니 Coding Style 서로 조금씩 달라서 PEP8 로 천하통일을 하려고 했습니다.

![PEP8 로 Coding Style 천하통일하기](http://newsmanager2.etomato.com/userfiles/image/%EC%B5%9C%EC%A4%80%ED%98%B8/1222_roll.png)

그래서 간단하게 PEP8 을 Jupyter Notebook 에 붙이는 방법에 대해 살펴보고자합니다.

---

# 1. PEP8

우선 [PEP8][2] 이 뭔가 살펴보겠습니다.

한국에서도 사람마다 말하는 방식이 다양합니다. 사람마다 주로 사용하는 단어가 있기도하고 연령, 지역별로 말하는 방식이 다르기도 합니다. 심한 방언의 경우 이해하기 어려운 경우도 있습니다.

![세상에는 이런 방식으로 말하는 사람도 있습니다](http://ppss.kr/wp-content/uploads/2016/07/%ED%99%8D%EC%A4%80%ED%91%9C_%EB%A7%89%EB%A7%90_%EB%85%BC%EB%9E%80_%EB%8F%84%EC%9D%98%EC%9B%90_%EC%93%B0%EB%A0%88%EA%B8%B0_%EA%B0%9C_7.jpg)

그래서 원활한 의사소통을 위해 공식적으로 표준 언어규범을 정하는데 바로 표준어입니다. 한국어에도 물론 표준어가 존재하고 아래와 같이 규정되어 있습니다.

> **대한민국 표준어**
> 표준어는 교양 있는 사람들이 두루 쓰는 현대 서울말로 정함을 원칙으로 한다. 

저는 이 규정을 볼때마다 `내가 과연 교양이 있는 사람인가...` 에 대한 고민에 빠지곤 합니다.

여튼 이런 표준어처럼 Python 의 세상에도 공식적으로 정한 표준 언어규범이 있는데, 그게 바로 PEP8 입니다. 프로그래머 마다의 습관이 있으니 실제 Coding Style 은 다를 수 있지만, 공동작업을 하거나 외부에 코드를 공유할 경우에는 이런 규칙이 정해져 있으면 편하게 사용할 수 있습니다.

---

# 2. Autopep8

그렇지만 우리가 표준어를 사용하기 위해, 표준어 규칙을 외우고 다니려면 참 피곤할 것 같습니다. 물론 중요한 표현이나 맞춤법은 숙지하고 있어야하지만 아나운서나 기자가 아닌 이상 완벽하게 알고 있기는 어렵습니다.

PEP8 도 마찬가지입니다. 프로그래머들이 본능적으로 일부 숙지하고 있지만 모든 규정을 완벽하게 외우기는 거의 불가능합니다.

이런 어려움을 돕기위해 자동으로 PEP8 을 적용해주는 착한 친구가 있으니 바로 [Autopep8][3] 입니다. pip 로 설치가 가능하고 conda 로도 설치가 가능합니다.

```bash
# pip 로 설치
$ pip install autopep8
```
또는
```bash
# conda 로 설치
$ conda install autopep8
```

간단하게 사용방법을 살펴보겠습니다.

우선 import 해서 python code 안에서 사용 할 수 있습니다.
```python
>>> import autopep8
>>> autopep8.fix_code("x=       1")
'x = 1\n'
```
혹은 파일에 바로 적용할 수도 있습니다.
```bash
$ echo "x=       1" > bad.py
$ autopep8 bad.py
x = 1
```

---

# 3. Jupyter Notebook 에 Autopep8 붙이기

이제 여기서부터 제가 이 포스트를 작성한 이유, Jupyter Notebook 에 Autopep8 을 붙이는 방법에 대해 살펴보겠습니다.

IPython 의 공식 [github wiki][4] 를 보면  `%%pep8`  라는 cell magic 이 나옵니다. 그리고 친절하게 [예제 Notebook][5] 도 작성되어 있습니다.

```ipython
%install_ext https://raw.githubusercontent.com/SiggyF/notebooks/master/pep8_magic.py
%load_ext pep8_magic
```
그런데 문제는, 여기서 사용한 `install_ext` 가 IPython 4.0 부터 Deprecated 되었고 5.0 부터는 아예 삭제되었습니다. 관심이 있으신분은 [#8634][6] 을 참고하시면 됩니다.

그래서 다른 방법을 찾아보던 중, [jupyter_contrib_nbextensions][7] 라는 좋은 친구를 발견하게 되었습니다.

아래와 같이 Jupyter Notebook 에서 nbextension 을 직접 관리 할 수 있게 도와줍니다. Autopep8 도 nbextension 형태로 제공되어 있어서 jupyter_contrib_nbextensions 를 사용하면 간편하게 Autopep8 을 붙일 수 있습니다.

![jupyter_contrib_nbextensions 의 우월한 관리화면](https://raw.githubusercontent.com/Jupyter-contrib/jupyter_nbextensions_configurator/master/src/jupyter_nbextensions_configurator/static/nbextensions_configurator/icon.png)

설치 방법은 pip 와 conda 모두 지원하고 있습니다.

```bash
# pip 로 설치
$ pip install jupyter_contrib_nbextensions
```
또는
```bash
# conda 로 설치
$ conda install -c conda-forge jupyter_contrib_nbextensions
```

그 다음에는 jupyter_contrib_nbextensions 에 필요한 Javascript 와 CSS 을 가져옵니다.
```bash
$ jupyter contrib nbextension install
```

마지막으로 Autopep8 을 enable 해보겠습니다. 여기서 당연한 이야기지만, Autopep8 를 위에서 설명한 방식대로 미리 설치두셔야 합니다.

Enable 은 간단하게 아래와 같이 NBextentions 에서 Autopep8 를 체크해주시면 됩니다.

![Autopep8 enable](/images/autopep8_enable.png)

단, 여기서 중요한 것은 위 스크린샷의 `compatibility: Jupyter (4.x)` 에서 보시는 것처럼 Jupyper 의 버전을 4.x 대로 맞춰야한다는 것입니다.

저의 경우 이미 5.x 를 사용하고 있었기 때문에 Enable 할 수 없었습니다. 그러나 특별히 5.x 의 기능 중에 당장 필요한 것이 없어서 4.x 로 버전을 내려왔습니다. 저는 4.3.0 버전으로 변경하였습니다.

```bash
# pip 로 설치
$ pip install jupyter_core==4.3.0
```
또는
```bash
# conda 로 설치
$ conda install jupyter_core==4.3.0
```

이제 여기까지 되시면 Jupyter Notebook 에서 Autopep8 를 사용하실 수 있습니다.

단축키를 사용하여 Autopep8 을 적용하는데 NBextentions 설정창에서 단축키를 확인, 변경할 수 있습니다.

제가 사용하는 Mac 기준으로는 `⌥A` 를 하시면 선택한 Cell 만, `⌥⇧A` 를 하시면 전체 Cell 에서 적용되니 참고하시기 바랍니다.

포스트를 참고해서 Jupyter Notebook 에 PEP8 을 적용해보시고 궁금하신 점이 있으시면 [JPark@JPark.me][mail_to_jpark] 로 언제든지 편하게 연락주시기 바랍니다.

[1]: http://jupyter.org/
[2]: https://www.python.org/dev/peps/pep-0008/
[3]: https://pypi.python.org/pypi/autopep8
[4]: https://github.com/ipython/ipython/wiki/Extensions-Index#pep8
[5]: http://nbviewer.jupyter.org/github/SiggyF/notebooks/blob/master/styleguide.ipynb
[6]: https://github.com/ipython/ipython/issues/8634
[7]: https://github.com/ipython-contrib/jupyter_contrib_nbextensions
[mail_to_jpark]: mailto:JPark@JPark.me
