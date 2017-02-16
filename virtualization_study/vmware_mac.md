Mac用のVMWare Fusionというらしい。
ちなみにWin用はVMWare Workstation Player。

# インストール

```sh
$ brew cask install vmware-fusion
```


# 起動

VMware Fusion内にあるvmrunコマンドを使用する。

```
$ /Applications/VMware\ Fusion.app/Contents/Library/vmrun start /path/to/vm.vmwarevm/vm.vmx
```

headlessモードで起動したい場合はnoguiをつける。

```
$ /Applications/VMware\ Fusion.app/Contents/Library/vmrun start /path/to/vm.vmwarevm/vm.vmx nogui
```

# vmdkファイルの結合

http://server-setting.info/blog/vmware-disk-file-marge.html

# vmdkファイルのリネーム

http://blog.dreamhive.co.jp/yama/12095.html#i-2
