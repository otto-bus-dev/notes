To install `yay` (Yet Another Yaourt), a popular AUR helper, on Arch Linux, follow these steps:

---

### 1. Install Prerequisites

First, make sure you have the base development tools and `git` installed:

bash

```
sudo pacman -S --needed base-devel git
```

---

### 2. Clone the `yay` Repository

bash

```
git clone https://aur.archlinux.org/yay.git
cd yay
```

---

### 3. Build and Install `yay`

bash

```
makepkg -si
```

- This command will build the package and install it.

---

### 4. Clean Up

Once installed, you can remove the `yay` build directory if you wish:

bash

```
cd ..
rm -rf yay
```

---

You can now use `yay` to install AUR and official packages, for example:

bash

```
yay -S <package_name>
```