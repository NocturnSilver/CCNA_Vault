## Ethernet Frame

| Length  | 7        | 1                           | 6          | 6          | 2                            |     | 4                          |
| ------- | -------- | --------------------------- | ---------- | ---------- | ---------------------------- | --- | -------------------------- |
| Section | Preamble | Start Frame Delimeter (SFD) | DST<br>MAC | SRC<br>MAC | length/Type                  | ... | Frame Check Sequence (FCS) |
| note    | 10101010 | 10101011                    |            |            | 1500 - length<br>1536 - Type |     |                            |
