### Entity와 DTO 설계
+ 처음에는 Builder 사용에 문제가 있을거라고 생각했었다.
+ 그런데, 이미지 업로드처리 도중에 문제가 발생해서 Builder null오류가 발생했던 것이였다.

### 생각
+ `다중 파일(이미지) 업로드에 Builder패턴 적용부분을 다시 시도해보자. `

### Entity 코드
```java
package excart.demoexcart.domain;

import excart.demoexcart.exception.NotEnoughStockException;
import lombok.Builder;
import lombok.Getter;

import javax.persistence.*;
import java.util.List;

@Entity
@Getter
@Table(name="item")
public class Item {

    @Id @GeneratedValue
    @Column
    private Long id;

    @Column
    private String name;

    @Column
    private int price;

    @Column
    private int stockQuantity;

    @Embedded
    @Column
    private List<UploadFile> imageFiles;

    public Item() {
    }

    @Builder(builderMethodName = "itemBuilder")
    public Item(String name, int price, int stockQuantity, List<UploadFile> imageFiles) {
        this.name = name;
        this.price = price;
        this.stockQuantity = stockQuantity;
        this.imageFiles = imageFiles;
    }


    @Builder
    public Item(Long id, String name, int price, int stockQuantity, List<UploadFile> imageFiles) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.stockQuantity = stockQuantity;
        this.imageFiles = imageFiles;
    }

    public Item toEntity() {
        return Item.itemBuilder()
                .name(name)
                .price(price)
                .stockQuantity(stockQuantity)
                .imageFiles(imageFiles)
                .build();
    }

    public void update(String name, int price, int stockQuantity) {
        this.name = name;
        this.price = price;
        this.stockQuantity = stockQuantity;
    }
    

//==비즈니스 로직==//
    /**
     * stock 증가
     */
    public void addStock(int quantity) {
        this.stockQuantity += quantity;
    }

    /**
     * stock 감소
     */
    public void removeStock(int quantity) {
        int restStock = this.stockQuantity - quantity;
        if (restStock < 0) {
            throw new NotEnoughStockException("need more stock");
        }
        this.stockQuantity = restStock;
    }

    @Override
    public String toString() {
        return "Item{" + "id=" + id + ", name=" + name + ", price=" + price + ", stockQuantity=" + stockQuantity + '}';
    }
}
```


### DTO 코드
```java
package excart.demoexcart.controller;

import excart.demoexcart.domain.Item;
import excart.demoexcart.file.FileStore;
import lombok.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.List;

@Getter
@Setter
@NoArgsConstructor
@ToString
public class RequestForm {

    private Long id;

    private String name;
    private int price;
    private int stockQuantity;
    private List<MultipartFile> imageFiles;

    @Autowired
    FileStore fileStore;

    public Item toEntity() throws IOException {
//        List<UploadFile> uploadFiles = fileStore.storeFiles(imageFiles);
        Item item = Item.itemBuilder()
                .name(name)
                .price(price)
                .stockQuantity(stockQuantity)
          //      .imageFiles(uploadFiles)
                .build();
        return item;
    }



    @Builder
    public RequestForm(Long id, String name, int price, int stockQuantity, List<MultipartFile> imageFiles) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.stockQuantity = stockQuantity;
        this.imageFiles = imageFiles;
    }

}
```

### 서비스 코드
```java
@RequiredArgsConstructor
@Service
@Slf4j
public class ItemService {

    private final ItemRepository itemRepository;

    @Transactional
    public Long save(RequestForm requestForm) throws IOException {
        return itemRepository.save(requestForm.toEntity()).getId();
    }
```

### 컨트롤러 코드
```java
    @GetMapping("/item/addItem")
    public String showAddForm(Model model) {
        log.info("상품생성폼으로 이동한다.");
        model.addAttribute("item", new Item());
        return "/item/add-item";
    }


    @PostMapping("/item/addItem")
    public String addItem(@ModelAttribute RequestForm form, RedirectAttributes redirectAttributes) throws IOException {

        Long savedId = itemService.save(form);

        redirectAttributes.addAttribute("itemId", savedId);
        redirectAttributes.addAttribute("status", true);

        log.info("@Post- form 읽기 ={}", form.getStockQuantity());
        Item itemEntity = itemService.findById(savedId);
        /*
        for (UploadFile imageFile : itemEntity.getImageFiles()) {
            log.info("이미지 파일명 읽기={}", imageFile.toString());
        }

         */

        return "redirect:/item-index";
    }
```
