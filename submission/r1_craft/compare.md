# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_006840.jpg
- L10 mid IGNORE_SCOPE
- L1 center SPURIOUS
- L5+R7 mid BOX_GEOMETRY
## adasind_036720.jpg
## adasind_056040.jpg
- L4 mid SPURIOUS
- L6 center SPURIOUS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 10 | 10 | 0 | 2 |
| mid | 7 | 6 | 1 | 2 |
| edge | 3 | 3 | 0 | 0 |
