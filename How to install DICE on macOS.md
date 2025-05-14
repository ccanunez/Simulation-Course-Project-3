To install DICE on macOS, follow these steps:

---

### 1. Install Required Dependencies

DICE requires the following software:

* CMake
* GSL (GNU Scientific Library)
* FFTW3 (Fastest Fourier Transform in the West)([GitHub][1])

You can install these using Homebrew:

```bash
brew install cmake gsl fftw
```

Alternatively, to install GSL and FFTW locally (recommended for cluster environments):

```bash
cd ~
mkdir local
cd local
```

Download and install GSL:

```bash
wget http://mirror.switch.ch/ftp/mirror/gnu/gsl/gsl-2.2.1.tar.gz
tar -xzf gsl-2.2.1.tar.gz
cd gsl-2.2.1
./configure --prefix=$HOME/local --enable-shared
make
make install
cd ..
```

Download and install FFTW:

```bash
wget http://www.fftw.org/fftw-3.3.5.tar.gz
tar -xzf fftw-3.3.5.tar.gz
cd fftw-3.3.5
./configure --prefix=$HOME/local --enable-threads --enable-shared
make
make install
cd ..
```

Add the local bin directory to your PATH by adding the following line to your `.profile`:

```bash
export PATH=$HOME/local/bin:$PATH
```

---

### 2. Clone the DICE Repository

```bash
git clone https://bitbucket.org/vperret/dice
```

---

### 3. Build and Install DICE

```bash
cd dice
mkdir build
cd build
cmake ..
make
make install
```

By default, DICE is installed in `$HOME/local/dice`. To specify a different installation directory, use:

```bash
cmake .. -DCMAKE_INSTALL_PREFIX=/desired/path
```

If you installed GSL and FFTW in non-standard locations, specify their paths:

```bash
cmake .. -DGSL_PATH=/path/to/gsl -DFFTW3_PATH=/path/to/fftw3 -DFFTW3_THREADS_PATH=/path/to/fftw3_threads
```

To enable threading for performance improvements:

```bash
cmake .. -DENABLE_THREADS=ON
make
make install
```

---

### 4. Update Your PATH

Add the DICE binary directory to your PATH by adding the following line to your `.profile`:

```bash
export PATH=$HOME/local/bin:$PATH
```

---

### 5. Notes

* Ensure that your CMake version is 2.6 or newer:

```bash
  cmake --version
```

* If you encounter errors related to `_intel_fast_memset_`, it may indicate that FFTW was compiled with the Intel compiler. In this case, you will need the Intel compiler to compile DICE.

* For GCC 10 and newer, if you encounter multiple definition errors, add the `-fcommon` flag:

```bash
  cmake .. -DCMAKE_C_FLAGS="-fcommon"
```

---

For more detailed information, refer to the DICE Compile & Install guide:&#x20;

---

[1]: https://github.com/perretv/dice?utm_source=chatgpt.com "perretv/dice - GitHub"

### You may encounter an error by CMakeLists.txt

The error you're seeing means that the `CMakeLists.txt` file for DICE is requesting compatibility with a very old CMake version (probably something like 2.6), which is no longer supported in modern versions of CMake (3.5+).

To fix this, you'll need to **edit the `CMakeLists.txt` file** in the DICE source directory.

---

### ✅ **Solution: Modify `CMakeLists.txt`**

1. Open the `CMakeLists.txt` file in the root of the DICE directory (i.e., `dice/CMakeLists.txt`).

2. Locate this line (likely near the top):

   ```cmake
   cmake_minimum_required(VERSION 2.6)
   ```

3. Change it to:

   ```cmake
   cmake_minimum_required(VERSION 3.5)
   ```

   This tells CMake to use at least version 3.5, which is compatible with modern systems.

4. Save the file.

---

### 🔁 Then Retry the Build

From your `build` directory, rerun:

```bash
cmake ..
make
make install
```

---

### What is $HOME and $PATH mean?

Great question! On macOS (and other Unix-like systems), `$HOME` and `$PATH` are **environment variables** that help your system know where to find files and executables.

---

### 🔍 What do `$HOME` and `$PATH` mean?

* **`$HOME`** is your **home directory** — for your user, it's usually:

  ```
  /Users/<your-username>
  ```

  You can check it by running:

  ```bash
  echo $HOME
  ```

* **`$PATH`** is a **colon-separated list of directories** that the system checks when you run a command in the terminal. For example, when you type `python`, the system searches in each directory listed in `$PATH` until it finds an executable named `python`.

---

### ✅ Step 4: Add DICE to Your PATH

Assuming DICE is installed in `~/local/bin` (which expands to `/Users/<your-username>/local/bin`), you want to tell your shell to include that directory when looking for commands.

#### 1. Open your terminal and edit your shell profile file:

Which file you edit depends on your shell:

| Shell                                      | File to Edit                |
| ------------------------------------------ | --------------------------- |
| `zsh` (default since macOS Catalina)       | `~/.zprofile` or `~/.zshrc` |
| `bash` (older versions or if manually set) | `~/.bash_profile`           |

To find out which shell you're using, run:

```bash
echo $SHELL
```

If you're using `zsh`, edit `~/.zprofile`:

```bash
nano ~/.zprofile
```

If you're using `bash`, edit `~/.bash_profile`:

```bash
nano ~/.bash_profile
```

#### 2. Add this line at the end of the file:

```bash
export PATH="$HOME/local/bin:$PATH"
```

This adds `~/local/bin` to the front of the existing `$PATH`.

#### 3. Save and exit:

* Press `Ctrl + O` to save
* Press `Enter` to confirm
* Press `Ctrl + X` to exit

#### 4. Reload the file (or restart the terminal):

```bash
source ~/.zprofile   # for zsh
# or
source ~/.bash_profile  # for bash
```

---

### ✅ Confirm it's working

Run:

```bash
which dice
```

If DICE was installed correctly and your path is updated, you should see something like:

```
/Users/<your-username>/local/bin/dice
```

### DICE should be working but how could you find it in your computer?

Great to hear DICE is working! 🚀 Let's clarify what happened with the files:

---

### 🧩 Where Are the DICE Files?

When you installed DICE, here’s what typically happens:

1. **You cloned the repository** using:

   ```bash
   git clone https://bitbucket.org/vperret/dice
   ```

   This created a folder called `dice` **in the directory where you ran that command** — most likely your home directory or another working directory in Terminal (you can check with `pwd` before cloning).

2. **You built the code** inside a subdirectory:

   ```
   dice/build
   ```

3. **You installed the compiled binary** using:

   ```bash
   make install
   ```

   This placed the DICE executable and related files into:

   ```
   ~/local/bin/        ← the main binary (executable)
   ~/local/lib/        ← if any libraries were installed
   ~/local/include/    ← headers (if any)
   ```

---

### 🧭 How to Find Them

#### 🔎 1. To find where the source files are:

In Terminal, run:

```bash
cd ~
ls -l | grep dice
```

You should see a `dice/` folder — that's the full source code and where you compiled DICE.

#### 📁 2. To view it in Finder:

In Terminal:

```bash
open ~/dice
```

#### 📂 3. To see the installed binary:

To open the folder where the binary went:

```bash
open ~/local/bin
```

You should see a file named `dice` there — that’s the actual program you run in the terminal.

---

### 💡 Tip

If you want, you can **move the source folder** (`~/dice`) to somewhere else (like `~/Documents/`) or delete it — the installed binary in `~/local/bin` will still work unless you need to recompile or modify it.

---

Yes, you can absolutely **copy the DICE folder** to another location — even multiple locations — and it won’t affect your ability to run DICE *as long as you don’t change the installed binary*. Here’s how it works:

---

### 🔁 **Copying vs. Running**

* The **folder `~/Downloads/dice/`** contains the **source code** and compiled object files.
* The **actual command you run** (`dice`) was installed in:

  ```
  ~/local/bin/dice
  ```

  That's the one your system uses when you type `dice` in the terminal — **not** the one in the `dice/build/` folder.

So yes: **copying or moving the `dice` folder won’t affect your ability to run DICE**, unless you delete the installed binary in `~/local/bin`.

---

### ✅ To copy the folder to another location:

For example, to copy it to `Documents`:

```bash
cp -R ~/Downloads/dice ~/Documents/dice_backup
```

You can open it in Finder to verify:

```bash
open ~/Documents/dice_backup
```

---

### ❌ When DICE *won’t* work

The command `dice` will **stop working** only if:

* You delete the binary: `rm ~/local/bin/dice`
* You uninstall FFTW or GSL (if it depends on them at runtime)
* You remove or unset the PATH update you added in `.zprofile` or `.bash_profile`

---

### 🧪 Try it!

You can test this now:

1. Copy the folder somewhere:

   ```bash
   cp -R ~/Downloads/dice ~/Desktop/dice_copy
   ```

2. Then in Terminal:

   ```bash
   dice
   ```

   It should still run normally ✔️

---


