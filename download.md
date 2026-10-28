# How to obtain

## Graphwar I

For Windows, you can download the game from [Steam](https://store.steampowered.com/app/1899700/Graphwar/).

Since there is a bug that makes the game fail to launch on Unix-like systems, such as Linux and macos, from Steam, you can download the game's jar from the [official website](https://www.graphwar.com/graphwar_1/graphwar.jar), or just tryna fix it by yourself XD. But don't download from the GitHub release, because the latest release was released 9 years ago.

If you want to use the latest version, whether Windows or Unix-like, you can build the game from the source code from the [GitHub](https://github.com/catabriga/graphwar) (JDK >= 8 and <= 25 is required).

```bash
git clone https://github.com/catabriga/graphwar.git
cd graphwar
chmod +x compile.sh
./compile.sh
# You'll get graphwar.jar in the current directory
```

If you are an Arch Linux user, you can use the AUR package [graphwar-git](https://aur.archlinux.org/packages/graphwar-git) to build the game, which is maintained by me. DO NOT use the package [graphwar](https://aur.archlinux.org/packages/graphwar), because it's outdated.

```bash
# For paru
paru -S graphwar-git
# For yay
yay -S graphwar-git
```

For the jar case, a Java Runtime Environment (JRE, version >= 8) is required, launch with the following command:

```bash
java -jar graphwar.jar # Or the path to the jar file
```

## Graphwar II

For PCs, you can buy the game from [Steam](https://store.steampowered.com/app/3066840/Graphwar_II/) or [itch.io](https://graphwar.itch.io/graphwar-ii).

For Android, the game is free to download from [Google Play](https://play.google.com/store/apps/details?id=com.graphwar.graphwar_ii), but the skins are not free.

For iOS, it's available to buy in the [App Store](https://apps.apple.com/us/app/graphwar-ii/id6517357575).