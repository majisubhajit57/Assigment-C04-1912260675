task1
struct Date { int day, month, year; };
struct BloodPressure { int systolic, diastolic; };
struct Patient {
    char name[20];
    int age;
    char phone[11];
    struct BloodPressure bp;
    struct Date visitDate;
};

// Repaired addPatient function
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
task2
// TODO 1
void print_parcel(const struct parcel *p) {
    printf("%d %.1f kg %s-%d %02d/%02d/%d\n", 
           p->id, p->weight, p->to.city, p->to.pincode, 
           p->booked.day, p->booked.month, p->booked.year);
}

// TODO 2
void sort_by_weight(struct parcel p[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (p[j].weight < p[min_idx].weight) {
                min_idx = j;
            }
        }
        struct parcel temp = p[i];
        p[i] = p[min_idx];

task3
void show(const struct reading *r) {
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
task4
void addInterest(struct account *a, float rate) {
    a->balance = a->balance + a->balance * rate / 100;
}
// Fix inside main loop:
addInterest(&list[i], 4);
// Fix in print statement:
printf("%d %f\n", p->number, p->balance);
task5
/* Issues the next mess token number, starting from 1001. */
int next_token(void) {
    // TODO 1 Completed: Static local variable retains value between calls
    static int token = 1000;
    return ++token;
}

/* The same logic with an automatic variable, for comparison. */
int auto_token(void) {
    int token = 1000; // Reinitialized every time
    return ++token;
}

// Register loop variable example (inside main)
// register int i;
// for(i = 0; i < 3; i++) { ... }
task6
#include <stdio.h>
#include <string.h>

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
        Date expiry;            // For PACKAGED
        Time prep_time;         // For COOKED
        int combo_ids[3];       // For COMBO
    } details;
} Item;

// Pseudocode/Implementation for Recording Sale
void record_sale(Item *item, int qty) {
    if (item->stock >= qty) {
        item->stock -= qty;
        item->revenue += (qty * item->price);
        printf("Sale: %dx %s. Remaining: %d\n", qty, item->name, item->stock);
    } else {
        printf("Error: Insufficient stock for %s\n", item->name);
    }
}

// Pseudocode/Implementation for Expiry Warning
void check_expiry(Item *item, Date today) {
    if (item->type == PACKAGED) {
        // Simplified check assuming same month/year for demonstration
        if (item->details.expiry.day - today.day <= 2) {
            printf("Warning: %s expires within 2 days!\n", item->name);
        }
    }
}

int main() {
    Item inventory[2] = {
        {1, "Chips", 20.0, 10, 0.0, PACKAGED, .details.expiry = {15, 11, 2026}},
        {2, "Samosa", 15.0, 5, 0.0, COOKED, .details.prep_time = {10, 30}}
    };
    Date today = {14, 11, 2026};

    // Demonstrate Normal Sale & Exceeding Stock
    record_sale(&inventory[1], 2); // Samosa
    record_sale(&inventory[1], 5); // Exceeds remaining 3

    // Demonstrate Expiry
    check_expiry(&inventory[0], today); // Chips expire on 15th, today is 14th

    return 0;
}
        p[min_idx] = temp;
    }
}
