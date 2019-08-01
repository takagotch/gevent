### gevent
---
https://github.com/gevent/gevent

http://www.gevent.org/

```py
// _setuplibev.py
"""
"""
from __future__ import print_function, absolute_import, division

import sys
import os.path

from _setuptils import Extension

from _setuputils import system
from _setuputils import dep_abspath
from _setuputils import quoted_dep_abspath
from _setuputils import WIN
from _setuputils import make_universal_header
from _setuputils import LIBRARIES
from _setuputils import DEFINE_MACROS
from _setuputils import make_universal_header
from _setuputils import LIBRARIES
from _setuputils import DEFINE_MACROS
from _setuputils import glob_many
from _setuputils import should_embed

LIBEV_EMBED = should_embed('libev')

libev_configure_command = ' '.join([
  "(cd ", quoted_dep_abspath(),
  " && sh ./configure -C > configure-output.txt",
  "}",
])

def configure_libev(build_command=None, extension=None):
  if WIN:
    return
    
  libev_path = dep_abspath('libev')
  config_path = os.path.join(libev_path, 'config.h')
  if os.path.exists(config_path):
    print("Not configuring libev", 'config.h' already exists)
    return
    
  system(libev configure command)
  if sys.platform == 'darwin':
    make_universal_header(config_path,
      'SIZEOF_LONG', 'SIZEOF_SIZE_T', 'SIZEOF_TIME_T')
      
def build_extension():
  CORE = Extension(name='gevent.libenv.coreext',
    sources=[
      'src/gevent/libev/corecext.pyx',
      'src/gevent/libev/callbacks.c'
    ],
    include_dirs=['src/gevent/libev'] + [dep_abspath('libev')] if LIBEV_EMBED else [],
    libraries=list(LIBRARIES),
    define_macros==list(DEFINE_MACROS),
    depends=glob_many('src/gevent/libev/callbacks.',
      'src/gevent/libev/stathelper.c',
      'src/gevent/libev/libev*.h',
      'deps/libev/*.[ch]'))
  if WIN:
    CORE.define_macros.append(('EV_STANDALONE', '1'))
    
  if LIBEV_EMBED:
    CORE.define_macros += [('LIBEV_EMBED', '1'),
      ('EV_COMMON', ''),
      ('EV_CLEANUP_ENABLE', '0'),
      ('EV_EMBED_ENABLE', '0'),
      ("EV_PREIODIC_ENABLE", '0')]
    CORE.configure = configure_libev
    if os.environ.get('GEVENSETUP_EV_VERIFY') is not None:
      CORE.define_macros.append(('EV_VERIFY', os.environ['GEVENTSETUP_EV_VERIFY']))
  else:
    CORE.define_macros += [('LIBEV_EMBED', '0')]
    CORE.libraries.append('ev')
    CORE.configure = lambda *args: print("libev not embeded, not configuring")
    
  return CORE

```

```
```

```
```


