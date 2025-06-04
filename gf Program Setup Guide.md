Great! You're almost there. Since you already have the `gf` executable in a folder, I’ll guide you step by step to **compile and run the full workflow** (including converting ASCII ICs, running `gf`, and reading the output) on your **macOS system**. You’ll need **gfortran**, and optionally tools like `mpifort` or `mpicc` if MPI is used later (though this version uses a dummy `mpif.f`, so no real MPI is needed).

---

## ✅ Step-by-Step Instructions on macOS

### **Step 0: Prerequisites**

Make sure you have **Homebrew** installed. If not, run this in Terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install **gfortran** (if not already installed):

```bash
brew install gcc
```

This installs `gfortran` as part of GCC.

Verify it works:

```bash
gfortran --version
```

---

### **Step 1: Organize Your Files**

Make a folder for the whole project:

```bash
mkdir ~/gf_project
cd ~/gf_project
```

Place the following source files into this folder:

* `gf.f` (the main N-body tree code)
* `mpif.f` (dummy MPI file)
* `ascii2drt.f` (for converting ASCII to binary)
* `snapread.f` (to read binary output)
* Your initial condition files in ASCII format (e.g., `stars.asc`, `gas.asc`, etc.)
* Your parameters file (e.g., `gi_par.asc`)

---

### **Step 2: Compile the Programs**

Compile `gf`:

```bash
gfortran -o gf gf.f mpif.f -O3 -ffixed-line-length-none
```

Compile the IC converter:

```bash
gfortran -o ascii2drt ascii2drt.f -O3 -ffixed-line-length-none
```

Compile the output reader:

```bash
gfortran -o snapread snapread.f -O3 -ffixed-line-length-none
```

Now you should have these executables:

* `gf`
* `ascii2drt`
* `snapread`

---

### **Step 3: Convert Initial Conditions to Binary**

Use `ascii2drt` to create the binary IC files. Example:

```bash
./ascii2drt stars.asc gr_str0000.drt
./ascii2drt gas.asc   gr_gas0000.drt
./ascii2drt dark.asc  gr_drk0000.drt
```

Make sure the output filenames match the expected format:

* `gr_str0000.drt`
* `gr_gas0000.drt`
* `gr_drk0000.drt`

---

### **Step 4: Run the Simulation**

Run `gf` using:

```bash
./gf &
```

Make sure `gi_par.asc` (the parameter file) and all the `gr_*0000.drt` files are in the same folder.

---

### **Step 5: Monitor Output**

As it runs, the following files will be generated:

* `go_log.asc` — log of time steps
* `go_dia.asc` — diagnostics
* Restart files (e.g., `gr_gas0200.drt`)
* Final outputs:

  * `gf_str.drt`
  * `gf_gas.drt`
  * `gf_drk.drt`

---

### **Step 6: Read Binary Output**

Use `snapread` to convert binary output to ASCII for analysis:

```bash
./snapread gf_str.drt output_str.asc
./snapread gf_gas.drt output_gas.asc
./snapread gf_drk.drt output_drk.asc
```

Now you'll have readable files of your simulation output.

---

## ✅ Optional Tips

* If any of your `.f` files have tabs or very long lines, you can open them in a text editor like **Visual Studio Code** or **BBEdit**.
* To stop the simulation at any time:

```bash
kill %1
```

---

# How do I stop the simulation?

Good question! When you run `./gf &`, you're starting the program **in the background**, and it's still running—so if you try to `exit`, the terminal warns you because there are **background jobs still active**.

Here’s how you can stop it properly:

---

### 🔍 Step 1: Check What’s Running

Run:

```bash
jobs
```

This will show something like:

```
[1]+  Running                 ./gf &
```

---

### ⛔ Step 2: Kill the Background Job

To **bring the job to the foreground** and stop it:

```bash
fg %1
```

Then press `Ctrl+C` to terminate it.

Alternatively, you can just kill it directly:

```bash
kill %1
```

Or, if it's not responding:

```bash
kill -9 %1
```

---

### ✅ Step 3: Confirm It’s Stopped

Run:

```bash
jobs
```

If you see nothing listed, the job is stopped, and it’s safe to `exit`.

---

Remember there are commands like `nohup` or `screen` so it survives even if you close the terminal.


