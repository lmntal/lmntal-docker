# Usage
## Exec program from stdin
```
$ docker run --rm -it lmntal/lmntal
```
Then write your LMNtal program to stdin, and put `C-d`,

## Exec from file
Create .lmn file containing LMNtal program.
```
$ cd #where program exists
$ docker run --rm -itv $PWD:/mnt slim [options] /mnt/program.lmn
```
or
```
$ cd #where program exists
$ docker rum --rm -itv $PWD:/mnt bash
```
Then bash appears, enjoy running `lmntal` and `slim`!
# Components
- `lmntal` - [LMNtal compiler](https://github.com/lmntal/lmntal-compiler)
- `slim` - [SLIM, the LMNtal runtime](https://github.com/lmntal/slim)

# Tags
- `latest`, `1.45-2.4.0` - [LMNtal compiler v1.45 and SLIM v2.4.0](https://github.com/lmntal/lmntal-docker/blob/master/1.45-2.4.0/Dockerfile)
- `1.44-2.3.1` - [LMNtal compiler v1.44 and SLIM v2.3.1](https://github.com/lmntal/lmntal-docker/blob/master/1.44-2.3.1/Dockerfile)

# LMNtal Information
For information about LMNtal, visit [official website](https://www.ueda.info.waseda.ac.jp/lmntal/index.php).
