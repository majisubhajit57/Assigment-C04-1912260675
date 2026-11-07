task1
#include <stdio.h>
#include <string.h>

struct Date { int day, month, year; };
struct BloodPressure { int systolic, diastolic; };

struct Patient {
    char name[20];
    int age;
    char phone[11];
    struct BloodPressure bp;
    struct Date visitDate;
};

void addPatient(struct Patient *p, const char n[], int a, const char ph[], int bps, int bpd, int d, int m, int y) {
    strcpy(p->name, n);
    p->age = a;
    strcpy(p->phone, ph);
    p->bp.systolic = bps;
    p->bp.diastolic = bpd;
    p->visitDate.day = d;
    p->visitDate.month = m;
    p->visitDate.year = y;
}

int main(void) {
    struct Patient p1;
    addPatient(&p1, "John Doe", 45, "9876543210", 120, 80, 15, 11, 2026);
    printf("Patient record added successfully.\n");
    return 0;
}
task2
#include <stdio.h>
#include <string.h>

struct date { int day, month, year; };
struct address { char city[20]; int pincode; };
struct parcel { int id; float weight; struct address to; struct date booked; };

void print_parcel(const struct parcel *p) {
    // TODO 1
    printf("%d %.1f kg %s-%d %02d/%02d/%d\n", 
           p->id, p->weight, p->to.city, p->to.pincode, 
           p->booked.day, p->booked.month, p->booked.year);
}

void sort_by_weight(struct parcel p[], int n) {
    // TODO 2
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (p[j].weight < p[min_idx].weight) {
                min_idx = j;
            }
        }
        struct parcel temp = p[i];
        p[i] = p[min_idx];
        p[min_idx] = temp;
    }
}

int main(void) {
    struct parcel list[3] = {
        {501, 4.5f, {"Madurai", 625001}, {12, 10, 2026}},
        {502, 1.2f, {"Chennai", 600077}, {12, 10, 2026}},
        {503, 2.8f, {"Coimbatore", 641001}, {13, 10, 2026}}
    };
    sort_by_weight(list, 3);
    for (int i = 0; i < 3; i++) print_parcel(&list[i]);
    return 0;
}

task3
#include <stdio.h>

enum kind { TEMPERATURE, HUMIDITY, STATUS };

struct reading {
    enum kind type; /* tag: tells which union member is valid */
    union {
        float celsius;
        int percent;
        char status[8];
    } value;
};

void show(const struct reading *r) {
    // TODO 1
    switch(r->type) {
        case TEMPERATURE:
            printf("Temp %.1f C\n", r->value.celsius);
            break;
        case HUMIDITY:
            printf("Humidity %d %%\n", r->value.percent);
            break;
        case STATUS:
            printf("Status %s\n", r->value.status);
            break;
    }
}

int main(void) {
    struct reading log[3] = {
        {TEMPERATURE, {.celsius = 31.5f}},
        {HUMIDITY, {.percent = 68}},
        {STATUS, {.status = "OK"}}
    };
    
    printf("sizeof union = %zu, sizeof struct reading = %zu\n",
           sizeof(log[0].value), sizeof(log[0]));
           
    for (int i = 0; i < 3; i++) show(&log[i]);
    return 0;
}
task4
#include <stdio.h>

struct account { int number; float balance; };

// Passed by pointer instead of value to fix defect
void addInterest(struct account *a, float rate) {
    a->balance = a->balance + a->balance * rate / 100;
}

int main(void) {
    struct account list[3] = {{101, 5000}, {102, 12000}, {103, 800}};
    struct account *p = list;
    
    for (int i = 0; i < 3; i++) {
        addInterest(&list[i], 4); // Fixed to pass address
    }
    
    // Fixed printf to use arrow operator
    for (int i = 0; i < 3; i++) {
        p = &list[i];
        printf("%d %f\n", p->number, p->balance);
    }
    return 0;
}
task5
#include <stdio.h>

/* Issues the next mess token number, starting from 1001. */
int next_token(void) {
    // TODO 1
    static int token = 1000;
    return ++token;
}

/* The same logic with an automatic variable, for comparison. */
int auto_token(void) {
    int token = 1000;
    return ++token;
}

int main(void) {
    int s1 = next_token(), s2 = next_token(), s3 = next_token();
    int a1 = auto_token(), a2 = auto_token(), a3 = auto_token();
    
    printf("static: %d %d %d\n", s1, s2, s3);
    printf("auto : %d %d %d\n", a1, a2, a3);
    return 0;
}
task6
#include <stdio.h>

typedef struct { int day, month, year; } Date;
typedef struct { int hour, minute; } Time;

enum ItemType { PACKAGED, COOKED, COMBO };

typedef struct {
    int id;
    char name[30];
    float price;
    int stock;
    float revenue;
    enum ItemType type;
    union {
        Date expiry;
        Time prep_time;
        int combo_ids[3];
    } details;
} Item;

void record_sale(Item *item, int qty) {
    if (item->stock >= qty) {
        item->stock -= qty;
        item->revenue += (qty * item->price);
        printf("Sale: %dx %s. Remaining: %d\n", qty, item->name, item->stock);
    } else {
        printf("Error: Insufficient stock for %s\n", item->name);
    }
}

void check_expiry(Item *item, Date today) {
    if (item->type == PACKAGED) {
        if (item->details.expiry.day - today.day <= 2) {
            printf("Warning: %s expires within 2 days!\n", item->name);
        }
    }
}

int main(void) {
    Item inventory[2] = {
        {1, "Chips", 20.0, 10, 0.0, PACKAGED, .details.expiry = {15, 11, 2026}},
        {2, "Samosa", 15.0, 5, 0.0, COOKED, .details.prep_time = {10, 30}}
    };
    Date today = {14, 11, 2026};

    record_sale(&inventory[1], 2);
    record_sale(&inventory[1], 5);
    check_expiry(&inventory[0], today);

    return 0;
}


    }
}
