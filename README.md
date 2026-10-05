BAI 2: CAU HINH PHAN QUYEN NHOM VA SUDOERS BANG VISUDO

MUC TIEU
- Biet cach tao tai khoan nguoi dung va nhom moi tren Linux.
- Hieu co che hoat dong cua dac quyen sudo va tep /etc/sudoers.
- Biet phan quyen gioi han cho mot nhom nguoi dung duoc thuc hien mot so lenh nhat dinh.

BOI CANH
Cap quyen restart dich vu he thong cho nhan vien Deployer tu dong hoa,
khong cap toan quyen root nham giam thieu rui ro an ninh.


1. TAO NHOM VA NGUOI DUNG MOI

Tao nhom devops-admin:

```bash
sudo groupadd devops-admin
```

Tao user deployer va them vao nhom devops-admin:

```bash
sudo adduser deployer
sudo usermod -aG devops-admin deployer
```

Kiem tra user va nhom:

```bash
id deployer
```

Ket qua mau:
uid=1001(deployer) gid=1001(deployer) groups=1001(deployer),1002(devops-admin)


2. CAU HINH SUDOERS BANG VISUDO

Mo file sudoers an toan:

```bash
sudo visudo
```

Them dong sau vao cuoi file (dau % bieu thi cho nhom):

```
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

Luu va thoat visudo (Ctrl+O, Enter, Ctrl+X neu dung nano).


3. KIEM TRA KET QUA

Chuyen sang tai khoan deployer:

```bash
su - deployer
```

Xem cac lenh sudo duoc phep:

```bash
sudo -l
```

Ket qua dau ra mau:

```
Matching Defaults entries for deployer on ubuntu-server:
    env_reset, mail_badpass, secure_path=...

User deployer may run the following commands on ubuntu-server:
    (ALL) NOPASSWD: /usr/bin/systemctl start *,
                    /usr/bin/systemctl stop *,
                    /usr/bin/systemctl restart *,
                    /usr/bin/systemctl status *
```

Chay thu lenh restart dich vu cron (khong can nhap mat khau):

```bash
sudo systemctl restart cron
```

Ket qua: Lenh chay thanh cong, khong xuat hien yeu cau nhap mat khau.


4. TONG KET CAC LENH DA THUC HIEN

```bash
sudo groupadd devops-admin
sudo adduser deployer
sudo usermod -aG devops-admin deployer
id deployer
sudo visudo
# (Them dong %devops-admin ALL=(ALL) NOPASSWD: ... vao cuoi file)
su - deployer
sudo -l
sudo systemctl restart cron
```


5. DONG CAU HINH SUDOERS DA THEM

```
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

Giai thich tung phan:
- %devops-admin : ap dung cho tat ca thanh vien nhom devops-admin.
- ALL=(ALL)     : tren moi may, duoc chay voi danh nghia bat ky user nao.
- NOPASSWD:     : khong yeu cau nhap mat khau.
- /usr/bin/systemctl start/stop/restart/status * : chi duoc phep chay 4 lenh nay voi moi tham so.
