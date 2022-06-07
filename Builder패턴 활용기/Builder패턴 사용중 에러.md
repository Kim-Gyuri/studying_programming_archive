### 에러
> `상품 등록요청 PostMapping 시 400에러 발생` <br> 
> ` DefaultHandlerExceptionResolver`  : <br>
Resolved [org.springframework.web.method.annotation.ModelAttributeMethodProcessor$1:  <br>
org.springframework.validation.BeanPropertyBindingResult:  <br>
1 errors<EOL> ` Field error in object 'requestForm' on field 'price': rejected value [null]; ` <br>
codes [typeMismatch.requestForm.price,typeMismatch.price,typeMismatch.int,typeMismatch];   <br>
arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [requestForm.price,price];  <br>
arguments []; default message [price]]; default message  <br>
[Failed to convert value of type 'null' to required type 'int';  <br>
nested exception is org.springframework.core.convert.ConversionFailedException:  <br>
Failed to convert from type [null] to type [int] for value 'null';  <br>
nested exception is java.lang.IllegalArgumentException: A null value cannot be assigned to a primitive type]]  <br>

<br>
  
### 해당 코드  
```java  
@PostMapping("/item/addItem")
    public String addItem(@ModelAttribute RequestForm form, RedirectAttributes redirectAttributes) throws IOException {

        List<UploadFile> uploadFiles = fileStore.storeFiles(form.getImageFiles());

        Item itemEntity = Item.itemBuilder()
                .name(form.getName())
                .price(form.getPrice())
                .stockQuantity(form.getStockQuantity())
                .imageFiles(uploadFiles)
                .build();

        Long savedId = itemService.save(itemEntity.toEntity());

        redirectAttributes.addAttribute("itemId", savedId);
        redirectAttributes.addAttribute("status", true);

        log.info("@Post- form 읽기 ={}", form.getStockQuantity());
        for (UploadFile imageFile : itemEntity.getImageFiles()) {
            log.info("이미지 파일명 읽기={}", imageFile.toString());
        }

        return "redirect:/item/show/{itemId}";
    }  
 ```
 
 + 요청객체 RequestForm form의 값을 꺼내 Item Buider생성하려고 할때, 값이 null로 거절되었다.
 + 그래서 요청거절되어 로그를 읽을 수도 없었다.
 
### 생각
+ Builder 사용과 객체요청을 제대로 이해 못해서 실패했다고 느꼈다.
