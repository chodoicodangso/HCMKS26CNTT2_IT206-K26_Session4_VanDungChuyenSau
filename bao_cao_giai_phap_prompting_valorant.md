# BAO CAO GIAI PHAP PROMPTING UNG DUNG: TU DONG HOA GIAO AN TAP AIM VALORANT

## 1. Thiet ke giai phap (Dau vao - Dau ra va Xu ly bay du lieu)

Giai phap su dung ky thuat noi chuoi cau lenh gom 2 buoc ket hop suy luan tung buoc de chuan doan loi ban va len lich tap luyen.

- Luong xu ly: Du lieu tho -> Cau lenh 1 (Loc va bat loi) -> Du lieu chuan JSON -> Cau lenh 2 (Suy luan tung buoc va len giao an) -> Ket qua cuoi cung.
- Dau vao: Tin nhan tu do cua nguoi choi gom muc hang, thong so chuot DPI, do nhay trong game, trieu chung ban truot.
- Dau ra: Du lieu JSON da kiem dinh o buoc 1, nhat ky suy luan tung buoc va giao an 30 phut moi ngay o buoc 2.
- Xu ly bay du lieu va ngat mach:
  1. Du lieu bi trong: Neu thieu DPI hoac do nhay, he thong gan trang thai THIEU_DU_LIEU, dung quy trinh va yeu cau nhap bo sung.
  2. Du lieu sai lech: Tinh chi so eDPI = DPI x Do nhay. Neu eDPI nho hon 80 hoac lon hon 1200 (do go nham dau thap phan), he thong gan trang thai SAI_THONG_SO, dung quy trinh va hoi xac nhan lai.

## 2. Chuoi cau lenh thuc thi

### Cau lenh 1: Trich xuat va kiem tra bay du lieu

```text
VAI TRO:
Ban la bo tien xu ly du lieu ky thuat Valorant. Nhiem vu la trich xuat thong so va bat loi tu tin nhan nguoi choi.

DU LIEU DAU VAO:
{{TIN_NHAN_NGUOI_CHOI}}

QUY TAC XU LY:
1. Trich xuat cac thong tin: muc_hang, dpi (so nguyen), do_nhay (so thuc), van_de_gap_phai (chuoi van ban). Truong nao thieu thi ghi null.
2. Dieu kien ngat mach:
   - Neu dpi la null hoac do_nhay la null -> trang_thai = "THIEU_DU_LIEU", edpi = null.
   - Tinh edpi = dpi * do_nhay. Neu edpi < 80 hoac edpi > 1200 -> trang_thai = "SAI_THONG_SO".
   - Cac truong hop con lai -> trang_thai = "HOP_LE".

DINH DANG DAU RA (Chi xuat duy nhat khoi JSON):
{"trang_thai": "HOP_LE|THIEU_DU_LIEU|SAI_THONG_SO", "muc_hang": "...", "dpi": 0, "do_nhay": 0, "edpi": 0, "van_de_gap_phai": "..."}
```

### Cau lenh 2: Suy luan tung buoc va xuat giao an

```text
VAI TRO:
Ban la Huan luyen vien Valorant. Nhan du lieu JSON tu Buoc 1 de chan doan nguyen nhan va len giao an 30 phut moi ngay.

DU LIEU DAU VAO:
{{KET_QUA_JSON_BUOC_1}}

QUY TRINH SUY LUAN TUNG BUOC:
- Buoc 1 (Kiem tra loi): Neu trang_thai khac "HOP_LE", DUNG len giao an, viet cau hoi yeu cau nguoi choi nhap lai hoac xac nhan thong so.
- Buoc 2 (Phan tich chi so eDPI neu HOP_LE): Muc chuan Valorant la 200 den 400. Neu eDPI > 450 la do nhay cao (de vay lo tam); neu eDPI < 180 la do nhay thap (vay thieu tam). Doi chieu voi van_de_gap_phai de tim nguyen nhan.
- Buoc 3 (Thiet ke giao an 30 phut): Chia lam 3 phan (10 phut Phong tap + 10 phut Phan mem luyen tay + 10 phut Dau tu) de khac phuc dung loi.

DINH DANG DAU RA:
Nhat ky suy luan: [Ghi ro tung buoc kiem tra]
Ket qua phan hoi: [Giao an chi tiet hoac Tin nhan bao loi]
```

## 3. Ket qua chay thu

### Truong hop 1: Du lieu hop le

- Dau vao: "Minh dang o hang Vang, dung chuot DPI 800, do nhay 0.65, ban toan bi vay lo qua dau dich."
- Ket qua Cau lenh 1:

```json
{"trang_thai": "HOP_LE", "muc_hang": "Vang", "dpi": 800, "do_nhay": 0.65, "edpi": 520, "van_de_gap_phai": "vay lo qua dau dich"}
```

- Ket qua Cau lenh 2:

```text
Nhat ky suy luan:
- Buoc 1: Trang thai HOP_LE, du dieu kien phan tich.
- Buoc 2: Chi so eDPI = 800 x 0.65 = 520 (lon hon muc chuan 450). Do nhay qua cao la nguyen nhan truc tiep khien tam bi vay lo qua dau dich. Can giam do nhay xuong muc 0.45 (tuong duong eDPI 360).
- Buoc 3: Thiet ke 3 bai tap ren kha nang dung tam va dat tam ngang dau trong 30 phut.

Ket qua phan hoi:
Ban hay giam do nhay trong game xuong 0.45 va tap theo lich 30 phut moi ngay duoi day:
1. Phong tap (10 phut): Chon che do tieu diet 50 muc tieu di chuyen ngang, tap dung han chuot roi moi ban tung vien bang sung Vandal.
2. Phan mem luyen tay (10 phut): Tap bai ban 6 muc tieu nho de luyen chinh tam chinh xac, chong vay lo.
3. Dau tu (10 phut): Chi su dung sung Guardian, tap trung dat tam ngang tam dau o cac goc mo.
```

### Truong hop 2: Du lieu bay (Go nham dau thap phan)

- Dau vao: "Cho minh xin giao an len hang Bach Kim, DPI 1600, do nhay 22."
- Ket qua Cau lenh 1:

```json
{"trang_thai": "SAI_THONG_SO", "muc_hang": "Bach Kim", "dpi": 1600, "do_nhay": 22, "edpi": 35200, "van_de_gap_phai": null}
```

- Ket qua Cau lenh 2:

```text
Nhat ky suy luan:
- Buoc 1: Phat hien trang thai SAI_THONG_SO do chi so eDPI = 35200 (vuot nguong toi da 1200). Kich hoat ngat mach an toan, khong xuat giao an sai lech.

Ket qua phan hoi:
Chi so eDPI cua ban dang la 35200, muc nay vuot qua gioi han thuc te. Co phai ban go nham tu 0.22 thanh 22 khong? Ban hay xac nhan lai do nhay chinh xac kem theo loi hay gap khi ban de minh len giao an chuan nhat nhe.
```