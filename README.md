import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
 
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
 
@SpringBootApplication
publicclassLibraryApplication {
 
   publicstaticvoidmain(String[] args) {
      SpringApplication.run(LibraryApplication.class, args);
   }
}
 
classBook {
   private String title;
   privateint copiesAvailable;
 
   publicBook(String title, int copiesAvailable) {
       this.title = title;
       this.copiesAvailable = copiesAvailable;
   }
 
   public String getTitle() {
       return title;
   }
 
   publicintgetCopiesAvailable() {
       return copiesAvailable;
   }
 
   publicvoiddecrementCopiesAvailable() {
       this.copiesAvailable--;
   }
}
 
classSubscriber {
   private String name;
   private List<String> subscribedBooks;
 
   publicSubscriber(String name) {
       this.name = name;
       this.subscribedBooks = newArrayList<>();
   }
 
   public String getName() {
       return name;
   }
 
   public List<String> getSubscribedBooks() {
       return subscribedBooks;
   }
 
   publicvoidaddSubscribedBook(String bookTitle) {
      subscribedBooks.add(bookTitle);
   }
}
 
@RestController
@RequestMapping("/library")
classLibraryController {
 
   private Map<String, Book> books = new HashMap<>();
   private Map<String, Subscriber> subscribers = new HashMap<>();
 
   @PostMapping("/addBook")
   public String addBook(@RequestBody Book book) {
      books.put(book.getTitle(), book);
       return"Book added successfully";
   }
 
   @GetMapping("/books")
   public Map<String, Book> getAllBooks() {
       return books;
   }
 
   @PostMapping("/subscribe/{subscriberName}/{bookTitle}")
   public String subscribeBook(@PathVariable String subscriberName, @PathVariable String bookTitle) {
       Subscribersubscriber= subscribers.computeIfAbsent(subscriberName, Subscriber::new);
       Bookbook= books.get(bookTitle);
 
       if (book == null) {
           return"Book not found";
       }
 
       if (book.getCopiesAvailable() > 0) {
          book.decrementCopiesAvailable();
          subscriber.addSubscribedBook(bookTitle);
           return"Subscription successful";
       } else {
           return"Book not available for subscription";
       }
   }
 
   @GetMapping("/subscribers")
   public Map<String, Subscriber> getAllSubscribers() {
       return subscribers;
   }
}
