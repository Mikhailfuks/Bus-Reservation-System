include <iostream>
include <string>
include <vector>
include <ctime>
include <iomanip>

using namespace std;

// Bus class
class Bus {
public:
    int busId;
    string destination;
    int capacity;
    int availableSeats;

    Bus(int busId, string destination, int capacity) {
        this->busId = busId;
        this->destination = destination;
        this->capacity = capacity;
        this->availableSeats = capacity;
    }
};

// Booking class
class Booking {
public:
    int bookingId;
    string customerName;
    Bus* bookedBus;
    int numSeats;
    time_t bookingDate;

    Booking(int bookingId, string customerName, Bus* bookedBus, int numSeats, time_t bookingDate) {
        this->bookingId = bookingId;
        this->customerName = customerName;
        this->bookedBus = bookedBus;
        this->numSeats = numSeats;
        this->bookingDate = bookingDate;
        bookedBus->availableSeats -= numSeats;
    }
};

// Bus Reservation System class
class BusReservationSystem {
private:
    vector<Bus> buses;
    vector<Booking> bookings;
    int nextBookingId = 1;

public:
    // Add a new bus to the system
    void addBus(int busId, string destination, int capacity) {
        buses.push_back(Bus(busId, destination, capacity));
    }

    // Display available buses
    void displayAvailableBuses() {
        cout << "Available Buses:" << endl;
        for (int i = 0; i < buses.size(); i++) {
            if (buses[i].availableSeats > 0) {
                cout << i + 1 << ". Bus ID: " << buses[i].busId << ", Destination: " << buses[i].destination << ", Available Seats: " << buses[i].availableSeats << endl;
            }
        }
    }

    // Book seats on a bus
    void bookSeats(int busIndex, string customerName, int numSeats) {
        if (busIndex >= 1 && busIndex <= buses.size() && numSeats > 0 && numSeats <= buses[busIndex - 1].availableSeats) {
            time_t bookingDate = time(0);
            bookings.push_back(Booking(nextBookingId++, customerName, &buses[busIndex - 1], numSeats, bookingDate));
            cout << "Booking successful!" << endl;
            cout << "Booking ID: " << bookings.back().bookingId << endl;
        } else {
            cout << "Invalid bus selection, insufficient seats, or invalid number of seats." << endl;
        }
    }

    // Cancel a booking
    void cancelBooking(int bookingId) {
        for (int i = 0; i < bookings.size(); i++) {
            if (bookings[i].bookingId == bookingId) {


