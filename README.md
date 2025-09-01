# gitman-issue

Demonstrates an issue that appeared in gitman v3.5.3 where the sparse_paths option no longer behaves as it should. The `gitman.yml` file in this repo has this set

```
sparse_paths:
    - a/
    - c
```

Which should copy only `a` and `c` from the https://github.com/filipopo/gitman-issue-files repo but instead, when you run

```
pipx run --spec gitman==3.5.3 --pip-args regex==2024.9.11 gitman update
```

You can observe that d also got copied (and sometimes the entire repo?)

```
ls gitman-issue-files/
a  c  d
```

Now delete the folder and run it with the older version

```
rm -rf gitman-issue-files/
pipx run --spec gitman==3.5.2 --pip-args regex==2024.9.11 gitman update
```

And observe that only `a` and `c` got copied, as is intended

```
ls gitman-issue-files/
a  c
```

The commands inject regex into the gitman venv due to a separate issue with the regex package not being pinned to a specific version and after some breaking changes gives this traceback when you run anything

<details>
  <summary>Traceback</summary>

```
Traceback (most recent call last):
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/sly/lex.py", line 311, in _build
    cpat = cls.regex_module.compile(part, cls.reflags)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/regex.py", line 353, in compile
    return _compile(pattern, flags, ignore_unused, kwargs, cache_pattern)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/regex.py", line 587, in _compile
    parsed = parsed.optimise(info, reverse)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3452, in optimise
    s = s.optimise(info, reverse)
        ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3008, in optimise
    subpattern = self.subpattern.optimise(info, reverse)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3452, in optimise
    s = s.optimise(info, reverse)
        ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3008, in optimise
    subpattern = self.subpattern.optimise(info, reverse)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3452, in optimise
    s = s.optimise(info, reverse)
        ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 2886, in optimise
    subpattern = self.subpattern.optimise(info, reverse)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3008, in optimise
    subpattern = self.subpattern.optimise(info, reverse)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3452, in optimise
    s = s.optimise(info, reverse)
        ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 3008, in optimise
    subpattern = self.subpattern.optimise(info, reverse)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 2089, in optimise
    prefix, branches = Branch._split_common_prefix(info, branches)
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 2202, in _split_common_prefix
    while pos < end_pos and prefix[pos].can_be_affix() and all(a[pos] ==
                                                           ^^^^^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 2202, in <genexpr>
    while pos < end_pos and prefix[pos].can_be_affix() and all(a[pos] ==
                                                               ^^^^^^^^^
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/regex/_regex_core.py", line 1938, in __eq__
    return type(self) is type(other) and self._key == other._key
                                         ^^^^^^^^^
AttributeError: 'AnyAll' object has no attribute '_key'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/home/mrs/.cache/pipx/11199b33660c49d/bin/gitman", line 3, in <module>
    from gitman.cli import main
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/gitman/__init__.py", line 3, in <module>
    from .commands import delete as uninstall
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/gitman/commands.py", line 10, in <module>
    from .models import Config, Source, find_nested_configs, load_config
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/gitman/models/__init__.py", line 2, in <module>
    from .config import Config, find_nested_configs, load_config
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/gitman/models/config.py", line 6, in <module>
    from datafiles import datafile, field
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/datafiles/__init__.py", line 6, in <module>
    from .decorators import auto, datafile, sync
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/datafiles/decorators.py", line 9, in <module>
    from .model import Model, create_model
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/datafiles/model.py", line 6, in <module>
    from . import config, hooks, settings
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/datafiles/hooks.py", line 8, in <module>
    from .mapper import create_mapper
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/datafiles/mapper.py", line 15, in <module>
    from . import config, formats, hooks
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/datafiles/formats.py", line 10, in <module>
    import json5
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/json5/__init__.py", line 1, in <module>
    from .dumper import dump
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/json5/dumper.py", line 11, in <module>
    from .loader import JsonIdentifier
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/json5/loader.py", line 10, in <module>
    from .model import BooleanLiteral
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/json5/model.py", line 10, in <module>
    from .tokenizer import JSON5Token
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/json5/tokenizer.py", line 61, in <module>
    class JSONLexer(Lexer):  # type: ignore[misc]
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/sly/lex.py", line 180, in __new__
    cls._build()
  File "/home/mrs/.cache/pipx/11199b33660c49d/lib/python3.12/site-packages/sly/lex.py", line 313, in _build
    raise PatternError(f'Invalid regex for token {tokname}') from e
sly.lex.PatternError: Invalid regex for token BLOCK_COMMENT
```

</details>
