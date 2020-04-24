---
layout: post
title: crepúsculo
comments: false
excerpt_separator: <!--more-->
---

Cuando quiero escribir una entrada voy a mi terminal y escribo: 

```
➜  ~ cd Documents/exterior/_posts 
➜  _posts git:(master) ✗ ./post.py
```

Esto abre `vim` en un archivo markdown con el nombre y preámbulo apropiado para que sea después procesado por [Jekyll](http://jekyllrb.com).
<!--more-->

El código de `post.py` va así:

{% highlight python %}
#!/usr/bin/env python

import subprocess
import os.path
import bisect
from datetime import datetime

PREAMBLE = """---
layout: post
title: {time}
comments: false
excerpt_separator: <!--more-->
---

"""

TIMES = [
    (0, 'trasnoche'),
    (4, 'madrugada'),
    (7, 'mañana'),
    (9, 'media mañana'),
    (11, 'mediodía'),
    (13, 'tarde'),
    (17, 'crepúsculo'),
    (20, 'noche')
]   


def generate_post():
    now = datetime.now()
    day = now.strftime('%Y-%m-%d')
    hour = now.hour
    breakpoints, times = zip(*TIMES)
    time = times[bisect.bisect(breakpoints, hour) - 1]
    file_name = f'{day}-{time}.md'.replace(' ', '-')
    file_content = PREAMBLE.format(time=time)
    if os.path.isfile(file_name):
        raise ValueError('Already a post during this period. Wait.')
    with open(file_name, 'w') as f:
        f.write(file_content)
    print('Post file created: {}'.format(file_name))
    subprocess.call(['vim', file_name])

if __name__ == '__main__':
    generate_post()
{% endhighlight %}

Probablemente hay una versión más bonita con un shellscript pero a mí me gusta usar python.

Otro día explico cómo genero el contenido. 
