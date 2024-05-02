#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>

void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j + 1] using a temporary variable
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main() {
    int numbers[] = {5, 4, 3, 2, 1};
    int n = sizeof(numbers) / sizeof(numbers[0]);

    pid_t pid = fork();

    if (pid < 0) {
        printf("Fork failed\n");
        return 1;
    } else if (pid == 0) {
        printf("Child process sorting...\n");
        bubbleSort(numbers, n);
        printf("Child process sorted the numbers\n");
        exit(0);
    } else {
        printf("Parent process waiting for child...\n");
        wait(NULL);
        printf("Parent process resumed.\n");

        printf("Sorted numbers by parent process: ");
        for (int i = 0; i < n; i++) {
            printf("%d ", numbers[i]);
        }
        printf("\n");

        sleep(5);
        printf("Parent process completed.\n");
    }

    return 0;
}
