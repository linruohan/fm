# fm

`fm` 是一个使用GTK和[Relm4]构建的小型通用文件管理器。

![Screenshot](https://user-images.githubusercontent.com/1372438/164090003-20bca431-e2ef-475a-86d4-df64d10e1989.png)

目录使用[Miller列]可视化，这使得快速在整个层次结构中导航。

`fm` 还处于发展的早期阶段。用它操纵重要文件存在数据丢失的风险。

## Platform support

目前的开发主要集中在Linux上，但也有针对其他平台的bug报告是受欢迎的。

已知该应用程序可以在MacOS上成功运行。其他平台未经测试，但如果您可以获得要构建的系统依赖项，那么‘ fm ’应该可以工作。

## Hacking

`fm` is a Rust project that utilizes [GTK 4][install-gtk],
[libpanel][install-libpanel], [GtkSourceView][install-gtksourceview], and
[libadwaita][install-libadwaita].

1. First, [install Rust and Cargo][install-rust].

2. Install system dependencies.

    Note that libpanel is alpha software and may not be packaged for your
    system. In that case, you can build it from source, install it, and then
    build `fm` with the `PKG_CONFIG_PATH` environment variable set to
    `PKG_CONFIG_PATH="/path/to/libpanel/lib/pkgconfig:$PKG_CONFIG_PATH"`.

    #### Arch Linux

    ```sh
    $ pacman -Syu gtk4 libadwaita libpanel-git gtksourceview5
    ```

    #### Fedora

    ```
    $ dnf install -y gtk4 libadwaita-devel libpanel
    ```

    #### openSUSE

    ```sh
    $ zypper in glib2-devel pango-devel gtk4-devel libadwaita-devel libpanel-devel gtksourceview5-devel libpoppler-glib-devel
    ```

3. Build and run the application.

    ```sh
    $ cargo run
    ```

## License

`fm` is licensed under the MIT license.

[Miller columns]: https://en.wikipedia.org/wiki/Miller_columns
[install-rust]: https://www.rust-lang.org/tools/install
[install-gtk]: https://www.gtk.org/docs/installations/
[install-gtksourceview]: https://wiki.gnome.org/Projects/GtkSourceView
[install-libadwaita]: https://gnome.pages.gitlab.gnome.org/libadwaita/
[install-libpanel]: https://gitlab.gnome.org/chergert/libpanel
[Relm4]: https://aaronerhardt.github.io/relm4-book/book/

## 编译
```bash
# 安装 GTK4 和依赖
pacman -S mingw-w64-x86_64-gtk4  mingw-w64-x86_64-libadwaita 
# 安装 gtksourceview-5
pacman -S mingw-w64-x86_64-gtksourceview5
# 安装poppler-sys-rs 依赖
pacman -S mingw-w64-x86_64-poppler
# 安装libpanel
pacman -S mingw-w64-x86_64-libpanel

```
## 编译问题
1. libpanel-0.5.0/src\auto/functions.rs:48:(.text+0x73fc): undefined reference to `panel_get_resource'␍
          collect2.exe: error: ld returned 1 exit status
```bash
That function is marked as internal.
$ rg panel_get_resource gtk-build/build/x64/release/libpanel/
gtk-build/build/x64/release/libpanel/_gvsbuild-meson\src\panel-resources.h
6:G_GNUC_INTERNAL GResource *panel_get_resource (void);

```
解决方案：修改为以下代码后清理后重新编译即可
C:\Users\Administrator\.cargo\registry\src\rsproxy.cn-e3de039b2554c837\libpanel-0.5.0\src\auto\functions.rs
```rs
#[doc(alias = "panel_get_resource")]
#[doc(alias = "get_resource")]
pub fn resource() -> Option<gio::Resource> {
    assert_initialized_main_thread!();
    // unsafe { from_glib_full(ffi::panel_get_resource()) }
    None
}
```
## 效果图
[!][image](./assets/01.png)