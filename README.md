# POCO X7 Pro / rodin GKI SukiSU Ultra build

GitHub Actions workflow для попытки сборки Android GKI kernel `android15-6.6`
arm64 с SukiSU Ultra, встроенным в kernel/GKI через официальный `setup.sh`
в режиме `builtin`.

## Device context

- Device: POCO X7 Pro / `rodin`
- ROM: `OS3.0.10.0.WOJMIXM`
- Android userspace: 16
- Current kernel: `6.6.89-android15-8-g8e4be6b47e40-ab14134548-4k`
- Page size: 4K
- Target: `android15-6.6` GKI arm64
- SukiSU Ultra mode: builtin, не LKM


// Туториал для чайников


## Как запустить Actions

1. Залей репозиторий на GitHub.
2. Открой вкладку `Actions`.
3. Выбери workflow `Build rodin android15-6.6 GKI with SukiSU Ultra`.
4. Нажми `Run workflow`.
5. Дождись окончания job `build-gki`.

Workflow скачивает Android common kernel workspace для `android15-6.6`,
применяет SukiSU Ultra через официальный `kernel/setup.sh builtin`, затем
пытается собрать GKI arm64 kernel.

## Где скачать artifact

1. Открой завершенный workflow run.
2. Пролистай страницу до блока `Artifacts`.
3. Скачай artifact `rodin-android15-6.6-sukisu-ultra-gki`.

Artifact может содержать найденные outputs:

- `Image`
- `*.ko`
- `*.img`
- `found-outputs.txt`

Если сборка упала до генерации kernel outputs, artifact может содержать только
диагностический список или отсутствовать.

## Важно про прошивку

Файл `Image` нельзя напрямую шить через `fastboot`.

Для прошивки нужен repack в `boot.img` на базе stock `boot.img` именно от
`OS3.0.10.0.WOJMIXM` для `rodin`. Используй stock `boot.img` как базу, замени
kernel image внутри него на собранный `Image`, затем прошивай уже repacked
`boot.img` вручную.

Workflow намеренно не содержит `fastboot` команд и ничего не прошивает.

## Откат

Если repacked boot не загрузился, откатись на заранее сохраненный backup:

```bash
fastboot flash boot_a boot3090_BACKUP.img
```
