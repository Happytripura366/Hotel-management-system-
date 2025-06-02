# Hotel-management-system-Hotel Management System :
import java.util.*;

// Abstract Staff class
abstract class Staff {
    protected String name;
    protected int id;
    protected String position;
    protected double salary;

    public Staff(String name, int id, String position, double salary) {
        this.name = name;
        this.id = id;
        this.position = position;
        this.salary = salary;
    }

    public String getDetails() {
        return id + ": " + name + " - " + position + ", Salary: $" + salary;
    }
}

// Receptionist subclass
class Receptionist extends Staff {
    public Receptionist(String name, int id, double salary) {
        super(name, id, "Receptionist", salary);
    }

    public void checkInGuest(Guest guest, Room room) {
        if (room.checkAvailability()) {
            room.bookRoom();
            guest.checkIn(room);
            System.out.println(guest.name + " checked into Room " + room.roomNumber);
        } else {
            System.out.println("Room not available.");
        }
    }

    public void checkOutGuest(Guest guest) {
        Room room = guest.getRoomBooked();
        if (room != null) {
            room.setStatus("Available");
            guest.checkOut();
            System.out.println(guest.name + " checked out of Room " + room.roomNumber);
        }
    }
}

// Manager subclass
class Manager extends Staff {
    public Manager(String name, int id, double salary) {
        super(name, id, "Manager", salary);
    }

    public void manageStaff(List<Staff> staffList) {
        System.out.println("Managing staff...");
        for (Staff s : staffList) {
            System.out.println(s.getDetails());
        }
    }

    public void manageRooms(List<Room> rooms) {
        System.out.println("Managing rooms...");
        for (Room room : rooms) {
            System.out.println("Room " + room.roomNumber + ": " + room.status);
        }
    }
}

// Guest class
class Guest {
    String name;
    int id;
    private Room roomBooked;

    public Guest(String name, int id) {
        this.name = name;
        this.id = id;
    }

    public void checkIn(Room room) {
        this.roomBooked = room;
    }

    public void checkOut() {
        this.roomBooked = null;
    }

    public Room getRoomBooked() {
        return roomBooked;
    }
}

// Room class
class Room {
    int roomNumber;
    String type;
    String status; // Available, Booked
    double price;

    public Room(int roomNumber, String type, double price) {
        this.roomNumber = roomNumber;
        this.type = type;
        this.status = "Available";
        this.price = price;
    }

    public boolean checkAvailability() {
        return status.equalsIgnoreCase("Available");
    }

    public void bookRoom() {
        this.status = "Booked";
    }

    public void setStatus(String status) {
        this.status = status;
    }
}

// Booking class
class Booking {
    int bookingID;
    int guestID;
    int roomNumber;
    Date checkInDate;
    Date checkOutDate;

    public Booking(int bookingID, int guestID, int roomNumber, Date checkIn, Date checkOut) {
        this.bookingID = bookingID;
        this.guestID = guestID;
        this.roomNumber = roomNumber;
        this.checkInDate = checkIn;
        this.checkOutDate = checkOut;
    }

    public void makeBooking() {
        System.out.println("Booking made for Guest ID " + guestID + " in Room " + roomNumber);
    }

    public void cancelBooking() {
        System.out.println("Booking cancelled for Guest ID " + guestID);
    }
}

// Hotel class
class Hotel {
    String name;
    String location;
    List<Room> rooms;
    List<Staff> staffList;

    public Hotel(String name, String location) {
        this.name = name;
        this.location = location;
        this.rooms = new ArrayList<>();
        this.staffList = new ArrayList<>();
    }

    public void addRoom(Room room) {
        rooms.add(room);
    }

    public void addStaff(Staff staff) {
        staffList.add(staff);
    }

    public void showRooms() {
        System.out.println("Rooms in " + name + ":");
        for (Room r : rooms) {
            System.out.println("Room " + r.roomNumber + " - " + r.type + " - " + r.status + " - $" + r.price);
        }
    }

    public void showStaff() {
        System.out.println("Staff at " + name + ":");
        for (Staff s : staffList) {
            System.out.println(s.getDetails());
        }
    }
}

// Main class to simulate hotel system
public class HotelSystem {
    public static void main(String[] args) {
        Hotel hotel = new Hotel("Seaview Resort", "Cox's Bazar");

        // Add rooms
        Room r1 = new Room(101, "Deluxe", 5000);
        Room r2 = new Room(102, "Standard", 3000);
        hotel.addRoom(r1);
        hotel.addRoom(r2);

        // Add staff
        Receptionist receptionist = new Receptionist("Mira", 1, 15000);
        Manager manager = new Manager("Rahim", 2, 30000);
        hotel.addStaff(receptionist);
        hotel.addStaff(manager);

        // Create guest and booking
        Guest guest = new Guest("Azim", 1001);

        // Show initial data
        hotel.showRooms();
        hotel.showStaff();

        // Receptionist handles check-in
        receptionist.checkInGuest(guest, r1);

        // Manager reviews system
        manager.manageStaff(hotel.staffList);
        manager.manageRooms(hotel.rooms);

        // Booking record
        Booking booking = new Booking(5001, guest.id, r1.roomNumber, new Date(), new Date());
        booking.makeBooking();

        // Checkout
        receptionist.checkOutGuest(guest);
        booking.cancelBooking();
    }
}
