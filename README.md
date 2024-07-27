# hotel-booking-app-
hello i have never ran into this error before can someone help that ? after entering " Import UIkit " I get an error that says " Declaration is only valid at file scope first time using GitHub too so I hope I am doing this right. Also, this is on the latest version of code .





import UIKit ( error occurs here )

// Room class
class Room {
    var roomNumber: Int
    var isBooked: Bool

    init(roomNumber: Int) {
        self.roomNumber = roomNumber
        self.isBooked = false
    }

    func book() {
        isBooked = true
    }

    func cancelBooking() {
        isBooked = false
    }
}

// Hotel class
class Hotel {
    var name: String
    var rooms: [Room]

    init(name: String) {
        self.name = name
        self.rooms = [
            Room(roomNumber: 101),
            Room(roomNumber: 102),
            Room(roomNumber: 103)
        ]
    }

    func getAvailableRooms() -> [Room] {
        return rooms.filter { !$0.isBooked }
    }

    func getRoom(byNumber roomNumber: Int) -> Room? {
        return rooms.first { $0.roomNumber == roomNumber }
    }
}

class ViewController: UIViewController {

    var hotel: Hotel!

    override func viewDidLoad() {
        super.viewDidLoad()

        hotel = Hotel(name: "Grand Hotel")

        // Set up UI elements programmatically if needed
    }

    @IBAction func viewRooms(_ sender: UIButton) {
        let availableRooms = hotel.getAvailableRooms()
        let message = availableRooms.isEmpty ? "No rooms available" : "Available Rooms: \(availableRooms.map { "\($0.roomNumber)" }.joined(separator: ", "))"
        showAlert(title: "Available Rooms", message: message)
    }

    @IBAction func bookRoom(_ sender: UIButton) {
        if let roomToBook = hotel.getAvailableRooms().first {
            roomToBook.book()
            showAlert(title: "Booking Successful", message: "Room \(roomToBook.roomNumber) has been booked.")
        } else {
            showAlert(title: "Booking Failed", message: "No rooms available for booking.")
        }
    }

    @IBAction func cancelBooking(_ sender: UIButton) {
        if let bookedRoom = hotel.rooms.first(where: { $0.isBooked }) {
            bookedRoom.cancelBooking()
            showAlert(title: "Booking Canceled", message: "Room \(bookedRoom.roomNumber) booking has been canceled.")
        } else {
            showAlert(title: "Cancel Failed", message: "No bookings to cancel.")
        }
    }

    private func showAlert(title: String, message: String) {
        let alert = UIAlertController(title: title, message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}
