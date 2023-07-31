# vim-encrypted-note-workflow
This is a vim encrypted note workflow.

Install fzf and nvim.

## Encrypt a whole existing directory

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

## Now you can decrypt the notes :
It will decrypt the file and open it in a vim editor. It re-encrypts it when you close it.
```
n () {
	decrypt_and_nvim $(find . -type f -name "*.enc" | fzf --preview "head {}")
}

decrypt_and_nvim() {
    if [[ -z "$1" ]]; then
        echo "Usage: decrypt_and_nvim <fichier.enc>"
    else
        decrypted_file="${1%.enc}"
        iterations=100000
        openssl enc -d -aes-256-cbc -pbkdf2 -iter "$iterations" -in "$1" -out "$decrypted_file" -k "VotreCleSecrete" && nvim "$decrypted_file"
        openssl enc -aes-256-cbc -pbkdf2 -iter "$iterations" -in "$decrypted_file" -out "$1" -k "VotreCleSecrete"
        rm "$decrypted_file"
    fi
}
```


