# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [shipping_api/stores/service.proto](#shipping_api_stores_service-proto)
    - [CreateRequest](#shipping_api-CreateRequest)
    - [CreateResponse](#shipping_api-CreateResponse)
    - [DeleteRequest](#shipping_api-DeleteRequest)
    - [DeleteResponse](#shipping_api-DeleteResponse)
    - [ListRequest](#shipping_api-ListRequest)
    - [ListResponse](#shipping_api-ListResponse)
    - [Store](#shipping_api-Store)
    - [UpdateRequest](#shipping_api-UpdateRequest)
    - [UpdateResponse](#shipping_api-UpdateResponse)
  
    - [StoresService](#shipping_api-StoresService)
  
- [Scalar Value Types](#scalar-value-types)



<a name="shipping_api_stores_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## shipping_api/stores/service.proto



<a name="shipping_api-CreateRequest"></a>

### CreateRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| store | [Store](#shipping_api-Store) |  |  |






<a name="shipping_api-CreateResponse"></a>

### CreateResponse







<a name="shipping_api-DeleteRequest"></a>

### DeleteRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| store_id | [string](#string) |  |  |






<a name="shipping_api-DeleteResponse"></a>

### DeleteResponse







<a name="shipping_api-ListRequest"></a>

### ListRequest







<a name="shipping_api-ListResponse"></a>

### ListResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| stores | [Store](#shipping_api-Store) | repeated |  |






<a name="shipping_api-Store"></a>

### Store
Store contains an ID and display name for a store


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |
| display_name | [string](#string) |  |  |






<a name="shipping_api-UpdateRequest"></a>

### UpdateRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| store_id | [string](#string) |  |  |
| store | [Store](#shipping_api-Store) |  |  |






<a name="shipping_api-UpdateResponse"></a>

### UpdateResponse






 

 

 


<a name="shipping_api-StoresService"></a>

### StoresService
StoresService provides a service for managing Stores
LATER: add links to guides around store definition and management

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| List | [ListRequest](#shipping_api-ListRequest) | [ListResponse](#shipping_api-ListResponse) | List retrieves a single listing by its ID |
| Create | [CreateRequest](#shipping_api-CreateRequest) | [CreateResponse](#shipping_api-CreateResponse) | Update creates a new store |
| Update | [UpdateRequest](#shipping_api-UpdateRequest) | [UpdateResponse](#shipping_api-UpdateResponse) | Update can be used to update existing stores |
| Delete | [DeleteRequest](#shipping_api-DeleteRequest) | [DeleteResponse](#shipping_api-DeleteResponse) | Delete can be used to delete existing stores |

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

