# vim-encrypted-note-workflow
This is a vim encrypted note workflow

### Encrypt a whole existing directory

### Input
```
📂 root
  ├─ 📁 dir1
  │    └─ 📄 file1.txt
  └─ 📁 dir2
       └─ 📄 file2.txt
```

```bash
your_secret_key="YOUR_SECRET_KEY"
for file in $(find . -type f -name "*.txt"); do
    openssl enc -aes-256-cbc -in "$file" -out "$file.enc" -k "$your_secret_key"
    rm "$file"
done
```

### Output
```
📂 root
  ├─ 📁 dir1
  │    └─ 🔒 file1.enc
  └─ 📁 dir2
       └─ 🔒 file2.enc
```
