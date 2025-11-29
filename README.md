extern "C" {

    // Multiboot header for GRUB
    __attribute__((section(".multiboot")))
    const unsigned long multiboot_header[] = {
        0x1BADB002,           // magic
        0x0,                  // flags
        -(0x1BADB002 + 0x0)   // checksum
    };

    // Entry point
    void _start() {
        const char* msg = "Welcome to My Simple C++ OS!";
        char* vga = (char*)0xB8000;

        for (int i = 0; msg[i] != '\0'; i++) {
            vga[i * 2] = msg[i];
            vga[i * 2 + 1] = 0x0F; // white color
        }

        while (1); // halt forever
    }
}
import java.util.*;

class Process {
    int pid, burst;

    Process(int pid, int burst) {
        this.pid = pid;
        this.burst = burst;
    }
}

public class MiniOS {

    Queue<Process> readyQueue = new LinkedList<>();

    // Add a process
    void addProcess(int pid, int burst) {
        readyQueue.add(new Process(pid, burst));
        System.out.println("Process " + pid + " added (Burst: " + burst + ")");
    }

    // CPU Scheduler (FCFS)
    void runScheduler() {
        System.out.println("\n--- CPU Scheduling Started (FCFS) ---");

        while (!readyQueue.isEmpty()) {
            Process p = readyQueue.poll();

            System.out.println("Running Process " + p.pid + " | Burst Time: " + p.burst);

            try {
                Thread.sleep(p.burst * 300); // Simulate CPU work
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("Process " + p.pid + " Finished.\n");
        }

        System.out.println("--- All Processes Completed ---");
    }

    public static void main(String[] args) {
        MiniOS os = new MiniOS();

        os.addProcess(1, 3);
        os.addProcess(2, 5);
        os.addProcess(3, 2);

        os.runScheduler();
    }
}
