1.Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.

Ищем через git show aefea

aefead220
Update CHANGELOG.md

2.Какому тегу соответствует коммит 85024d3?

Ищем git show 85024d3

commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)

3.Сколько родителей у коммита b8d720? Напишите их хеши.

Ищем через мержи git show b8d720

commit b8d720f8340221f2146e4e4870bf2ee0bc48f2d5
Merge: 56cd7859e0 9ea88f22fc
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Tue Jan 21 17:45:48 2020 -0800

commit 56cd7859e05c36c06b56d013b55a252d0bb7e158     56cd7859e0
commit 9ea88f22fc6269854151c571162c5bcf958bee2b     9ea88f22fc

4.Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

Сортируем через git log  v0.12.23..v0.12.24  --oneline

33ff1c03bb (tag: v0.12.24) v0.12.24
b14b74c493 [Website] vmc provider links
3f235065b9 Update CHANGELOG.md
6ae64e247b registry: Fix panic when server is unreachable
5c619ca1ba website: Remove links to the getting started guide's old location
06275647e2 Update CHANGELOG.md
d5f9411f51 command: Fix bug when using terraform login on Windows
4b6d06cc5d Update CHANGELOG.md
dd01a35078 Update CHANGELOG.md
225466bc3e Cleanup after v0.12.23 release

5.Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).

Ищем git log -S'func providerSource' --oneline, git show 5af1e6234a, git show 8c928e8358

5af1e6234a main: Honor explicit provider_installation CLI config when present
8c928e8358 main: Consult local directories as potential mirrors of providers

commit 5af1e6234ab6da412fb8637393c5a17a1b293663
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Tue Apr 21 16:28:59 2020 -0700

commit 8c928e83589d90a031f811fae52a81be7153e82f
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Thu Apr 2 18:04:39 2020 -0700

6.Найдите все коммиты в которых была изменена функция globalPluginDirs.

Запрашиваем лог  git log -L :'func globalPluginDirs':plugins.go --oneline

plugins.go:func globalPluginDirs() []string {

78b1220558 Remove config.go and update things using its aliases

diff --git a/plugins.go b/plugins.go
--- a/plugins.go
+++ b/plugins.go
@@ -16,14 +18,14 @@
 func globalPluginDirs() []string {
        var ret []string
        // Look in ~/.terraform.d/plugins/ , or its equivalent on non-UNIX
-       dir, err := ConfigDir()
+       dir, err := cliconfig.ConfigDir()
        if err != nil {
                log.Printf("[ERROR] Error finding global config directory: %s", err)
        } else {
                machineDir := fmt.Sprintf("%s_%s", runtime.GOOS, runtime.GOARCH)
                ret = append(ret, filepath.Join(dir, "plugins"))
                ret = append(ret, filepath.Join(dir, "plugins", machineDir))
        }

        return ret
 }
52dbf94834 keep .terraform.d/plugins for discovery

diff --git a/plugins.go b/plugins.go
--- a/plugins.go
+++ b/plugins.go
@@ -16,13 +16,14 @@
 func globalPluginDirs() []string {
        var ret []string
        // Look in ~/.terraform.d/plugins/ , or its equivalent on non-UNIX
        dir, err := ConfigDir()
        if err != nil {
                log.Printf("[ERROR] Error finding global config directory: %s", err)
        } else {
                machineDir := fmt.Sprintf("%s_%s", runtime.GOOS, runtime.GOARCH)
+               ret = append(ret, filepath.Join(dir, "plugins"))
                ret = append(ret, filepath.Join(dir, "plugins", machineDir))
        }

        return ret
 }
41ab0aef7a Add missing OS_ARCH dir to global plugin paths

diff --git a/plugins.go b/plugins.go
--- a/plugins.go
+++ b/plugins.go
@@ -14,12 +16,13 @@
 func globalPluginDirs() []string {
        var ret []string
        // Look in ~/.terraform.d/plugins/ , or its equivalent on non-UNIX
        dir, err := ConfigDir()
        if err != nil {
                log.Printf("[ERROR] Error finding global config directory: %s", err)
        } else {
-               ret = append(ret, filepath.Join(dir, "plugins"))
+               machineDir := fmt.Sprintf("%s_%s", runtime.GOOS, runtime.GOARCH)

7.Кто автор функции synchronizedWriters?

Сортируем через git log -S'func synchronizedWriters' --pretty=format:'%h - %an %ae'

bdfea50cc8 - James Bardin j.bardin@gmail.com
5ac311e2a9 - Martin Atkins mart@degeneration.co.uk

