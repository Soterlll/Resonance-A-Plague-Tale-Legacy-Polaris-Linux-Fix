# Resonance: A Plague Tale Legacy on AMD Polaris (RX 470/480/570/580/590) — Linux / Proton

Патченный **vkd3d-proton v3.1** (две DLL), который позволяет пройти Resonance на картах Polaris под Proton.
Без подмены шейдеров, без прокси-DLL (`dxgi.dll`, шим), без `.ini`, без правок папки игры (кроме двух DLL).

A patched **vkd3d-proton v3.1** (two DLLs) that lets Resonance run on Polaris GPUs under Proton.
No shader replacement, no proxy DLLs, no ini files.

## Что делает / What it does
- Для `Resonance.exe` на Polaris (AMD, по PCI DeviceID) включает профиль совместимости: FL 12_1, SM 6.6,
  Native16BitShaderOpsSupported, эмуляция VRS Tier 2 (VRS игнорируется), wave64-совместимость.
- ROV: у RADV на Polaris нет `VK_EXT_fragment_shader_interlock`. Интерлок вырезается из SPIR-V, ROV работает как
  обычный UAV (порядок записи не гарантируется). В прохождении артефактов замечено не было.
- 16-битные compute-шейдеры не отвергаются на Polaris.
- Игнорирует чужие CachedPSO-блобы (`pipeline_library_ignore_mismatch_driver`).
- Эмуляция ExecuteIndirect из патча njtechyt включается только если у драйвера нет нативного DGC
  (на современном RADV нативный DGC есть, эмуляция не используется).

## Проверено / Tested
RX 580 8GB (Sapphire Nitro+, Polaris 20, `0x67DF`), CachyOS, Heroic. Proton: GE-Proton11-12 и Proton-CachyOS
(финал пройден на Proton-CachyOS). Mesa 26.2.x и 26.3.1 (драйверный FP16 на gfx8 для vkd3d включён),
ядра 6.18.60 LTS и 7.3.5. Игра пройдена целиком, около 45 FPS. **Не проверялось:** другие карты Polaris,
Steam-версия, другие дистрибутивы и версии Proton.

## Установка / Install
1. Скопируй `bin/d3d12.dll` и `bin/d3d12core.dll` в папку с `Resonance.exe`. Обе DLL из одной сборки.
2. Переменная окружения: `WINEDLLOVERRIDES="d3d12,d3d12core=n,b"` (в Heroic: Advanced → Environment Variables;
   в Steam: параметры запуска `WINEDLLOVERRIDES="d3d12,d3d12core=n,b" %command%`).
3. Первый запуск долгий: игра компилирует пайплайны (десятки тысяч вариантов), потом кэш ускоряет запуск.

Опционально / Optional env:
- `VKD3D_POLARIS_GFX_MIN16_AS_32=1` — min16float в графических стадиях считать как 32-бит (по умолчанию выключено).
- `VKD3D_POLARIS_DISABLE_EXTENSIONS=1` — отключить mutable_descriptor_type, descriptor_buffer, module_identifier
  (нужно только на драйверах Windows-типа, на RADV не нужно).
- `VKD3D_LOG_FILE=...` / `VKD3D_DEBUG=warn` — журнал. В логе есть строки `POLARIS: ...` (определение карты, форсирование возможностей, вырезание интерлока).

## Известные проблемы / Known issues
- **Ядро Linux 7.3.1 (linux-cachyos) ломает Polaris**: чёрный экран, `Failed to pin framebuffer: -EINVAL`
  (CachyOS/linux-cachyos issue #1127). На 7.3.5 и LTS 6.18 игра работала.
- Нестабильный разгон/андервольт карты даёт зависания; на штатных частотах проблем не было.
- `breadcrumbs` в этой сборке недоступен (нужна сборка с `-Denable_trace=true`).

## Сборка из исходников / Build
```
git clone --branch v3.1 --recurse-submodules https://github.com/HansKristian-Work/vkd3d-proton.git
cd vkd3d-proton && git apply /path/to/patch/RX580-resonance-v3.1.patch
meson setup --cross-file build-win64.txt --buildtype release -Denable_tests=false -Denable_extras=false build
ninja -C build     # d3d12.dll, d3d12core.dll
```
(На старом mingw-w64 может понадобиться `-Dc_args=-DPATHCCH_NONE=0`.)

## Лицензия / License
LGPL-2.1-or-later (как vkd3d-proton). См. `LICENSE` и `NOTICE.md`.
