package com.smartparking;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import java.util.*;

@SpringBootApplication
public class SmartParkingApplication {
    public static void main(String[] args) {
        SpringApplication.run(SmartParkingApplication.class, args);
    }
}

@RestController
@RequestMapping("/api")
class ParkingController {
    private final Map<Integer, Boolean> parkingSlots = new HashMap<>();
    
    public ParkingController() {
        for (int i = 1; i <= 10; i++) {
            parkingSlots.put(i, true); // true means available
        }
    }
    
    @GetMapping("/status")
    public String getParkingStatus() {
        return "Parking system is running";
    }
    
    @GetMapping("/slots")
    public Map<Integer, Boolean> getAvailableSlots() {
        return parkingSlots;
    }
    
    @PostMapping("/book/{slotId}")
    public String bookSlot(@PathVariable int slotId) {
        if (parkingSlots.containsKey(slotId) && parkingSlots.get(slotId)) {
            parkingSlots.put(slotId, false);
            return "Slot " + slotId + " booked successfully.";
        }
        return "Slot " + slotId + " is not available.";
    }
    
    @PostMapping("/release/{slotId}")
    public String releaseSlot(@PathVariable int slotId) {
        if (parkingSlots.containsKey(slotId) && !parkingSlots.get(slotId)) {
            parkingSlots.put(slotId, true);
            return "Slot " + slotId + " released successfully.";
        }
        return "Slot " + slotId + " is already available.";
    }
}

@RestController
@RequestMapping("/api/auth")
class AuthController {
    private final Map<String, String> users = new HashMap<>(); // Simulated user storage
    
    @PostMapping("/signup")
    public String signup(@RequestParam String username, @RequestParam String password) {
        if (users.containsKey(username)) {
            return "Username already exists.";
        }
        users.put(username, password);
        return "User registered successfully.";
    }
    
    @PostMapping("/login")
    public String login(@RequestParam String username, @RequestParam String password) {
        if (users.containsKey(username) && users.get(username).equals(password)) {
            return "Login successful.";
        }
        return "Invalid credentials.";
    }
}
