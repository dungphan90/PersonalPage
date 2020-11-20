## Tensorflow with CUDA support
1. Install NVIDIA driver, `CUDA`, and `CUDNN` as described in [here](setting-up-archlinux.md).
2. Setup base and virtual python environment as in [here](setting-up-archlinux.md). You can look at my sample setup functions below. These two function can be put in `~/.zshrc` or `~/.zprofile` (or `~/.bashrc` and `~/.profile` if you are using `bash`).
```bash
# Using PyEnv
function setup_pyenv() {
  export PYENV_ROOT="$HOME/.pyenv"
  export PATH="$PYENV_ROOT/bin:$PATH"
  if command -v pyenv 1>/dev/null 2>&1; then
    eval "$(pyenv init -)"
  fi
}

function setup_cudasupport_python() {
  setup_pyenv
  source $HOME/Pyenvs/cudasupport/bin/activate
}
```
3. You have 2 choices: using `python-tensorflow-opt-cuda` prebuilt and maintained by AUR, or build `tensorflow` and maintain (by maintaining, I just mean updating the source and rebuilding it when needed, not like writing fixes and patches) a `tensorflow` package locally, on your own. A caveat of the first choice is that you will have to use the native python, which I always highly recommend people not to touch. If you want to do it anyway, you can easily install the package with `yay`.
```bash
yay -S python-tensorflow-opt-cuda
```
If you want to build `tensorflow` from the source. Follow through the next steps. Notice that, from here, we are following the official tensorflow website's [instructions](https://www.tensorflow.org/install/source).
4. Now you have a virtual python environment called `cudasupport`, go ahead and set it up (open a new terminal, locate and run the `activate` script corresponding to the environment, or run `setup_cuda_support` if you chose to use my setup functions mentioned earlier).

We will need to upgrade `pip` and `wheel` to the latest version, install `numpy` without `--upgrade` and `keras_preprocessing` with `--no-deps`. Notice: if you are not using virtual environment, adding `--user` option.
```bash
pip install --upgrade pip setuptools wheel
pip install numpy
pip install keras_preprocessing --no-deps
```
5. Getting the source code. I make a folder in my virtual environment base directory (same level as `bin`, `lib` and `include`) and name it `tensorflow-build`. This is the folder that I will use to keep the `tensorflow`'s source code.
```bash
cd ./tensorflow-build # Notice that my current folder is virtual environment base dir
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
git checkout r2.4
```
I choose the `r2.4` release here. In my case, I can successfully build it with `gcc` (7.5.0), `CUDA` (11.1.105), `CUDNN` (8.0.5), `TensorRT` (7.0) (you will not need `TensorRT` for your laptop or personal workstation, I just want to see if my build can work with `TensorRT` 7.0), `bazel` (3.1.0), base `python` (3.7.3), and `numpy` (1.19.4). In the first try to build it with `gcc` (10.2.0) which is the default version on Arch release (2020.11.01), I encounter a lot of issues. I will post them here later. So, please install `gcc-7` with `yay` before you proceed. 

To be continued...

