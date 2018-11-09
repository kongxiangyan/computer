## 安装Jupyter Notebook

:link: [官方文档](https://jupyter.readthedocs.io/en/latest/index.html)

### Install

While Jupyter runs code in many programming languages, **Python** is a requirement (Python 3.3 or greater, or Python 2.7) for installing the Jupyter Notebook.

First, ensure that you have the latest `pip`; older versions may have trouble with some dependencies:

```bash
pip3 install --upgrade pip
```

Then install the Jupyter Notebook using:

```bash
pip3 install jupyter
# 或者
pip install jupyter
```

```bash
# 安装完毕，查看版本
jupyter --version
jupyter notebook --version
```

```bash
# Start the notebook server from the command line:
jupyter notebook
# You should see the notebook open in your browser.
```

When the notebook opens in your browser, you will see the Notebook Dashboard, which will show a list of the notebooks, files, and subdirectories in the directory where the notebook server was started. Most of the time, you will wish to start a notebook server in the highest level directory containing notebooks. **Often this will be your home directory.**

```bash
# 站点目录
D:\Software\Programming\Python3.7\Lib\site-packages\notebook\static
```

### Command Options

```bash
# The following code should open the given notebook in the currently running notebook server, starting one if necessary.
jupyter notebook ${notebook.ipynb}

# By default, the notebook server starts on port 8888. If port 8888 is unavailable or in use, the notebook server searches the next available port. You may also specify a port manually. In this example, we set the server’s port to 9999:
jupyter notebook --port 9999

# Start notebook server without opening a web browser:
jupyter notebook --no-browser

# The notebook server provides help messages for other command line arguments using the --help flag:
jupyter notebook --help
```

### Configuration

```bash
# To create a default config file, run:
jupyter ${application} --generate-config
# The generated file will be named jupyter_${application}_config.py.
# 注意终端的提示，像下面这样：
Writing default config to: C:\Users\cigaret\.jupyter\jupyter_notebook_config.py

# 打开该配置文件，找到如下内容：
## The directory to use for notebooks and kernels.
# c.NotebookApp.notebook_dir = ''
# 自定义notebook的文件路径，并取消注释，结果如下：
c.NotebookApp.notebook_dir = 'D:\GitLocal\PythonGit\IPYNB'

# 此部分修改jupyter notebook的默认启动端口，初始为8888
## The port the notebook server will listen on.
#c.NotebookApp.port = 8888

# 如下是open_browser选项，默认为True，即在运行jupyter notebook之后，自动打开浏览器，可以改为 False 禁用该选项，之后在浏览器中输入 localhost:8888 （端口视你自己的配置而定，初始为8888）打开
## Whether to open in a browser after starting. The specific browser used is
#  platform dependent and determined by the python standard library `webbrowser`
#  module, unless it is overridden using the --browser (NotebookApp.browser)
#  configuration option.
#c.NotebookApp.open_browser = True
c.NotebookApp.open_browser = False
```

### Extensions

#### Jupyter notebook extensions

:link: [扩展的Github仓库](https://github.com/ipython-contrib/jupyter_contrib_nbextensions)

:link: [该扩展的文档](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/index.html)

```bash
# 1. Install the python package
pip install jupyter_contrib_nbextensions
# 2. Install javascript and css files
jupyter contrib nbextension install --user
```

#### Jupyter Nbextensions Configurator

:link: [扩展的Github仓库](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator)

```bash
# 1. Installing the pip package
pip install jupyter_nbextensions_configurator
# 2. Configuring the notebook server to load the server extension. A jupyter subcommand is provided for this.
jupyter nbextensions_configurator enable --user
```

Once `jupyter_nbextensions_configurator` is installed and enabled, and your notebook server has been restarted, you should be able to find the nbextensions configuration interface at the url `<base_url>nbextensions`, where `<base_url>` is described below (for simple installs, it's usually just `/`, so the UI is at `/nbextensions`).

```bash
# just like
localhost:8888/nbextensions
```

## 安装\升级为JupyterLab

:link: [官方手册](https://jupyterlab.readthedocs.io/en/latest/index.html)

### Install

JupyterLab requires the Jupyter Notebook version 4.3 or later. To check the version of the `notebook` package that you have installed:

```bash
jupyter notebook --version
```

```bash
# 使用pip安装jupyterlab
pip install jupyterlab
```

If installing using `pip install --user`, you must add the user-level `bin` directory to your `PATH`environment variable in order to launch `jupyter lab`.

If you are using a version of Jupyter Notebook earlier than 5.3, then you must also run the following command to enable the JupyterLab server extension:

```bash
jupyter serverextension enable --py jupyterlab --sys-prefix
```

```bash
# 后台启动jupyter lab
nohup jupyter-lab > /home/ubuntu/software/jupyter/log/jupyterlab.log &
# ps 查看进程
# kill 结束进程
```

### Starting JupyterLab

```bash
# Start JupyterLab using:
jupyter lab # JupyterLab will open automatically in your browser.
```

To open the classic Notebook from JupyterLab, select “Launch Classic Notebook” from the JupyterLab Help menu, or you can change the URL from `/lab` to `/tree`.

### Extensions

#### Install & Uninstall


```bash
# query the current application path
jupyter lab path # /usr/local/share/jupyter/lab

# You can install new extensions into the application using the command:
jupyter labextension install ${my-extension}
# where my-extension is the name of a valid JupyterLab extension npm package on npm. Use the my-extension@version syntax to install a specific version of an extension, for example:
jupyter labextension install ${my-extension@1.2.3}

# You can list the currently installed extensions by running the command:
jupyter labextension list

# Uninstall an extension by running the command:
jupyter labextension uninstall ${my-extension}
```

You can install/uninstall more than one extension in the same command by listing their names after the `install` command.

If you are installing/uninstalling several extensions in several stages, you may want to defer rebuilding the application by including the flag `--no-build` in the install/uninstall step. 

```bash
# Once you are ready to rebuild, you can run the command:
jupyter lab build
```

#### Enable & Disable

```bash
# You can disable specific JupyterLab extensions (including core extensions) without rebuilding the application by running the command:
jupyter labextension disable ${my-extension} # This will prevent the extension from loading in the browser, but does not require a rebuild.

# You can re-enable an extension using the command:
jupyter labextension enable ${my-extension}
```

#### Extensions

##### Jupyterlab discovery

 :link: [Github](https://github.com/vidartf/jupyterlab_discovery)

A JupyterLab extension to facilitate the discovery and installation of other extensions.

> Note: This extension has now been included in the core of JupyterLab! To enable it:

- Go into advanced settings editor.
- Open the Extension Manager section.
- Add the entry `"enabled": true`.
- Save the settings.
- If prompted whether you are sure, read the warning, and click "Enable" if you're still sure ;)

##### jupyterlab_materialdarker

:link: [Github](https://github.com/oriolmirosa/jupyterlab_materialdarker)

```bash
jupyter labextension install @oriolmirosa/jupyterlab_materialdarker
jupyter labextension install @oriolmirosa/jupyterlab_materialdarker --no-build
```

##### jupyteralb-toc

:link: [Github](https://github.com/ian-r-rose/jupyterlab-toc)

```bash
jupyter labextension install @jupyterlab/toc
jupyter labextension install @jupyterlab/toc --no-build
```

##### JupyterLab LaTex

:link: [Github](https://github.com/jupyterlab/jupyterlab-latex)

This extension includes both a notebook server extension (which interfaces with the LaTeX compiler) and a lab extension (which provides the UI for the LaTeX preview). In order to use it, you must enable both of them.

```bash
# To install the server extension, run the following in your terminal:
pip install jupyterlab_latex
# If you are running Notebook 5.2 or earlier, enable the server extension by running
jupyter serverextension enable --sys-prefix jupyterlab_latex
# To install the lab extension, run
jupyter labextension install @jupyterlab/latex

# The extension defaults to running `xelatex` on the server. This command may be customized (e.g., to use pdflatex instead) by customizing your `jupyter_notebook_config.py` file:
c.LatexConfig.latex_command = 'pdflatex'
```

> `pip install jupyterlab_latex` 需要Python 3.7

##### jupyterlab_voyager

:link: [Github](https://github.com/altair-viz/jupyterlab_voyager)

```bash
jupyter labextension install jupyterlab_voyager
jupyter labextension install jupyterlab_voyager --no-build
```

### kernels

:link: [Github](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)

## 进阶配置

[搭建ipython/jupyter notebook 服务器](https://bitmingw.com/2017/07/09/run-jupyter-notebook-server/)

[jupyter服务器搭建](https://aitical.github.io/2018/09/02/jupyter%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%90%AD%E5%BB%BA/)

## Others

- [awesome-jupyter](https://github.com/markusschanta/awesome-jupyter) - A curated list of awesome [Jupyter](http://jupyter.org/) projects, libraries and resources. Jupyter is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text.

- [Rodeo](https://rodeo.yhat.com/) - Native Python IDE for Data Science.
- [Stencila](https://github.com/stencila/stencila) - Native desktop notebook frontend.
- [jupyter-drive](https://github.com/jupyter/jupyter-drive) - Google drive for Jupyter.
- [JuliaBox](https://www.juliabox.com/) - Run Julia in your Browser