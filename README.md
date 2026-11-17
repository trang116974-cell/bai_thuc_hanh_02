# bai_thuc_hanh_02
## Bai 1:
#include <stdio.h>
#include <stdlib.h>
void nhapMang(float *a, int n) {
    for (int i = 0; i < n; i++) {
        printf("Nhap phan tu thu %d: ", i);
        scanf("%f", a + i); 
    }
}
void inMang(float *a, int n) {
    for (int i = 0; i < n; i++) {
        printf("%.2f  ", *(a + i)); 
    }
    printf("\n");
}
int main() {
    float *a;
    int n;
    printf("Nhap so luong phan tu n: ");
    scanf("%d", &n);
    if (n <= 0) {
        printf("So luong phan tu phai lon hon 0!\n");
        return 1;
    }
    a = (float*) malloc(n * sizeof(float));
    if (a == NULL) {
        printf("Khong du bo nho de cap phat!\n");
        return 1;
    }
    printf("\n--- NHAP MANG ---\n");
    nhapMang(a, n);
    printf("\n--- MANG VUA NHAP ---\n");
    inMang(a, n);
    free(a);
    a = NULL;
    return 0;
}
## Bai 2:
#include <stdio.h>
#include <stdlib.h>
void nhapMang2Chieu(float **a, int hang, int cot) {
    for (int i = 0; i < hang; i++) {
        for (int j = 0; j < cot; j++) {
            printf("Nhap a[%d][%d]: ", i, j);
            scanf("%f", *(a + i) + j); 
        }
    }
}
void inMang2Chieu(float **a, int hang, int cot) {
    for (int i = 0; i < hang; i++) {
        for (int j = 0; j < cot; j++) {
            printf("%8.2f", *(*(a + i) + j)); 
        }
        printf("\n"); // Xuống dòng sau khi in hết một hàng
    }
}

int main() {
    float **a;
    int hang, cot;
    printf("Nhap so hang: ");
    scanf("%d", &hang);
    printf("Nhap so cot: ");
    scanf("%d", &cot);
    if (hang <= 0 || cot <= 0) {
        printf("So hang va so cot phai lon hon 0!\n");
        return 1;
    }
    a = (float**) malloc(hang * sizeof(float*));
    for (int i = 0; i < hang; i++) {
        *(a + i) = (float*) malloc(cot * sizeof(float));
    }
    printf("\n--- NHAP MA TRAN SO THUC ---\n");
    nhapMang2Chieu(a, hang, cot);
    printf("\n--- MA TRAN VUA NHAP ---\n");
    inMang2Chieu(a, hang, cot);
    for (int i = 0; i < hang; i++) {
        free(*(a + i));
    }
    free(a);
    a = NULL;
    return 0;
}
## Bai 4:
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct {
    char hoten[30];
    int masv;
    float d1, d2, d3;
} Data;
typedef struct tagNode {
    Data infor;
    struct tagNode *link;
} Node;
typedef struct {
    Node *pHead;
    Node *pTail;
    int spt;
} LList;
void xoaBoDem(void) {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}
Node* taoNode(Data x) {
    Node *pp = (Node*)malloc(sizeof(Node));
    if (pp == NULL) return NULL;
    pp->infor = x;
    pp->link = NULL;
    return pp;
}
void initList(LList *L) {
    L->pHead = NULL;
    L->pTail = NULL;
    L->spt = 0;
}
int emptyList(LList L) {
    return (L.spt == 0);
}
/* 1. Thêm vào đầu */
void insert_Head(LList *L, Data x) {
    Node *pp = taoNode(x);
    if (pp == NULL) { printf("Khong du bo nho!\n"); return; }
    if (emptyList(*L)) {
        L->pHead = pp;
        L->pTail = pp;
    } else {
        pp->link = L->pHead;
        L->pHead = pp;
    }
    L->spt++;
}
void insert_Tail(LList *L, Data x) {
    Node *pp = taoNode(x);
    if (pp == NULL) { printf("Khong du bo nho!\n"); return; }
    if (emptyList(*L)) {
        L->pHead = pp;
        L->pTail = pp;
    } else {
        L->pTail->link = pp;
        L->pTail = pp;
    }
    L->spt++;
}
int insert_After(LList *L, Data x, int maCho) {
    Node *q = L->pHead;
    while (q != NULL && q->infor.masv != maCho) q = q->link;
    if (q == NULL) return 0;
    Node *pp = taoNode(x);
    if (pp == NULL) { printf("Khong du bo nho!\n"); return 0; }
    pp->link = q->link;
    q->link = pp;
    if (L->pTail == q) L->pTail = pp;
    L->spt++;
    return 1;
}
void insert_Sorted(LList *L, Data x) {
    Node *pp = taoNode(x);
    if (pp == NULL) { printf("Khong du bo nho!\n"); return; }
    if (emptyList(*L) || L->pHead->infor.masv >= x.masv) {
        pp->link = L->pHead;
        L->pHead = pp;
        if (L->pTail == NULL) L->pTail = pp;
        L->spt++;
        return;
    }
    Node *q = L->pHead;
    while (q->link != NULL && q->link->infor.masv < x.masv) q = q->link;
    pp->link = q->link;
    q->link = pp;
    if (L->pTail == q) L->pTail = pp;
    L->spt++;
}
int remove_First(LList *L) {
    if (emptyList(*L)) return 0;
    Node *p = L->pHead;
    L->pHead = p->link;
    if (L->pHead == NULL) L->pTail = NULL;
    free(p);
    L->spt--;
    return 1;
}
int remove_Last(LList *L) {
    if (emptyList(*L)) return 0;
    if (L->pHead == L->pTail) return remove_First(L);
    Node *p = L->pHead;
    while (p->link != L->pTail) p = p->link;
    free(L->pTail);
    p->link = NULL;
    L->pTail = p;
    L->spt--;
    return 1;
}
int remove_ByMa(LList *L, int maCho) {
    if (emptyList(*L)) return 0;
    if (L->pHead->infor.masv == maCho) return remove_First(L);
    Node *q = L->pHead;
    while (q->link != NULL && q->link->infor.masv != maCho) q = q->link;
    if (q->link == NULL) return 0;
    Node *p = q->link;
    q->link = p->link;
    if (L->pTail == p) L->pTail = q;
    free(p);
    L->spt--;
    return 1;
}
Node* search_ByMa(LList L, int maCho) {
    for (Node *p = L.pHead; p != NULL; p = p->link)
        if (p->infor.masv == maCho) return p;
    return NULL;
}
void nhap(Data *x) {
    printf("Nhap ma sv: ");
    scanf("%d", &(x->masv));
    xoaBoDem();
    if (x->masv <= 0) return;
    printf("Nhap ho ten: ");
    fgets(x->hoten, sizeof(x->hoten), stdin);
    x->hoten[strcspn(x->hoten, "\n")] = '\0';
    printf("Nhap diem mon 1 2 3 (cach nhau khoang trang): ");
    scanf("%f %f %f", &(x->d1), &(x->d2), &(x->d3));
    xoaBoDem();
}
void in(Data x) {
    float dtb = (x.d1 + x.d2 + x.d3) / 3.0f;
    printf("Ten: %-25s Ma: %-6d Diem 1: %5.2f  Diem 2: %5.2f  Diem 3: %5.2f  DTB: %5.2f\n",
           x.hoten, x.masv, x.d1, x.d2, x.d3, dtb);
}
void processList(LList L) {
    if (emptyList(L)) { printf("Danh sach rong!\n"); return; }
    printf("\n--- DANH SACH SINH VIEN (%d) ---\n", L.spt);
    Node *pp = L.pHead;
    while (pp != NULL) {
        in(pp->infor);
        pp = pp->link;
    }
}
void freeList(LList *L) {
    Node *p = L->pHead;
    while (p != NULL) {
        Node *next = p->link;
        free(p);
        p = next;
    }
    L->pHead = L->pTail = NULL;
    L->spt = 0;
}
int main(void) {
    LList L;
    int kt = 0;
    initList(&L);
    do {
        printf("\n===== QUAN LY SINH VIEN - DSLK DON =====\n");
        printf("1. Them nut vao dau danh sach\n");
        printf("2. Them nut vao cuoi danh sach\n");
        printf("3. Them nut vao sau mot nut khac\n");
        printf("4. Them sinh vien vao danh sach sap xep\n");
        printf("5. Tim kiem theo ma SV\n");
        printf("6. Xoa sinh vien theo ma SV\n");
        printf("7. Xoa nut dau\n");
        printf("8. Xoa nut cuoi\n");
        printf("9. Duyet danh sach\n");
        printf("0. Thoat\n");
        printf("Moi ban chon: ");
        int chon;
        if (scanf("%d", &chon) != 1) { xoaBoDem(); chon = -1; }
        else xoaBoDem();
        Data x;
        int maCho;
        switch (chon) {
        case 1:
            printf("Nhap thong tin sv can them:\n");
            nhap(&x);
            if (x.masv > 0) insert_Head(&L, x);
            break;
        case 2:
            printf("Nhap thong tin sv can them:\n");
            nhap(&x);
            if (x.masv > 0) insert_Tail(&L, x);
            break;
        case 3:
            printf("Nhap thong tin sv can them:\n");
            nhap(&x);
            if (x.masv > 0) {
                printf("Them sau sv co ma: ");
                scanf("%d", &maCho);
                xoaBoDem();
                if (!insert_After(&L, x, maCho))
                    printf("Khong tim thay sv ma %d\n", maCho);
            }
            break;
        case 4:
            printf("Nhap thong tin sv can them:\n");
            nhap(&x);
            if (x.masv > 0) insert_Sorted(&L, x);
            break;
        case 5: {
            printf("Nhap ma sv can tim: ");
            scanf("%d", &maCho);
            xoaBoDem();
            Node *p = search_ByMa(L, maCho);
            if (p) in(p->infor);
            else printf("Khong ton tai sv ma %d\n", maCho);
            break;
        }
        case 6:
            printf("Nhap ma sv can xoa: ");
            scanf("%d", &maCho);
            xoaBoDem();
            if (remove_ByMa(&L, maCho)) printf("Da xoa sv ma %d\n", maCho);
            else printf("Khong tim thay sv ma %d\n", maCho);
            break;
        case 7:
            if (!remove_First(&L)) printf("Danh sach rong!\n");
            break;
        case 8:
            if (!remove_Last(&L)) printf("Danh sach rong!\n");
            break;
        case 9:
            processList(L);
            break;
        case 0:
            printf("Bye bye\n");
            kt = 1;
            break;
        default:
            printf("Lua chon khong hop le!\n");
        }
    } while (kt == 0);

    freeList(&L);
    return 0;
}
