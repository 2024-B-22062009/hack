#include <stdio.h>
#include <string.h>

#define MAX_REQUESTS 100

typedef struct {
    int id;
    char name[50];
    char category[20];
    char location[100];
    int quantity;
    char status[20];
} WasteRequest;

WasteRequest requests[MAX_REQUESTS];
int requestCount = 0;

void addRequest() {
    if (requestCount >= MAX_REQUESTS) {
        printf("Maximum number of requests reached.\n");
        return;
    }

    WasteRequest req;
    req.id = requestCount + 1;

    printf("Enter waste name: ");
    scanf(" %[^\n]", req.name);

    printf("Enter category (organic/recyclable/hazardous/others): ");
    scanf(" %[^\n]", req.category);

    printf("Enter location: ");
    scanf(" %[^\n]", req.location);

    printf("Enter quantity: ");
    scanf("%d", &req.quantity);

    strcpy(req.status, "pending");

    requests[requestCount++] = req;
    printf("✅ Request added successfully! ID: %d\n\n", req.id);
}

void viewRequests() {
    printf("\n📋 All Waste Collection Requests:\n");
    if (requestCount == 0) {
        printf("No requests yet.\n");
        return;
    }

    for (int i = 0; i < requestCount; i++) {
        WasteRequest req = requests[i];
        printf("ID: %d | Name: %s | Category: %s | Location: %s | Qty: %d | Status: %s\n",
               req.id, req.name, req.category, req.location, req.quantity, req.status);
    }
    printf("\n");
}

void updateStatus() {
    int id;
    char newStatus[20];
    printf("Enter request ID to update: ");
    scanf("%d", &id);

    int found = 0;
    for (int i = 0; i < requestCount; i++) {
        if (requests[i].id == id) {
            printf("Enter new status (pending/collected): ");
            scanf(" %[^\n]", newStatus);
            strcpy(requests[i].status, newStatus);
            printf("✅ Status updated for request ID %d\n\n", id);
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("❌ Request with ID %d not found.\n\n", id);
    }
}

int main() {
    int choice;

    do {
        printf("=== Waste Collection System ===\n");
        printf("1. Add Collection Request\n");
        printf("2. View All Requests\n");
        printf("3. Update Request Status\n");
        printf("0. Exit\n");
        printf("Choose an option: ");
        scanf("%d", &choice);
        printf("\n");

        switch (choice) {
            case 1: addRequest(); break;
            case 2: viewRequests(); break;
            case 3: updateStatus(); break;
            case 0: printf("Exiting... Bye!\n"); break;
            default: printf("Invalid choice. Try again.\n");
        }

    } while (choice != 0);

    return 0;
}
