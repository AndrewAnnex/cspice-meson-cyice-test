# cspice meson test for cython

```bash
# something like...

meson setup build --reconfigure

ninja -C build -v
pip install dist/test_cyice-0.0.1-cp313-cp313-macosx_15_0_arm64.whl
```