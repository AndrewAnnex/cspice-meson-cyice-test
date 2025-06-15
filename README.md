# cspice meson test for cython for spiceypy proof of concept

```bash
# something like...

meson setup build --reconfigure
meson compile -C build #(or ninja -C build -v)

python -m build -w .

pip install dist/test_cyice-0.0.1-cp313-cp313-macosx_15_0_arm64.whl
```