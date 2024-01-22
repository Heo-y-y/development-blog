애플리케이션에서 데이터를 처리하는 방법은 여러가지가 있다. 해당 앱에서 보면 여행 상품을 계산하는 데이터를 처리해줘야 한다. 여행 상품을 추가 할 수록 그에 맞게 가격을 올려줘야하고, 옵션을 추가 해도 올려줘야 한다. 그리고 총 가격을 상품 주문 페이지에서도 보고 주문 확인 페이지에서 봐야하기 때문에 데이터를 여러곳에 이동해주어야 한다.

![스크린샷1 2024-01-22 오후 5 02 21](https://github.com/Heo-y-y/development-blog/assets/112863029/8e63b71a-786c-43f4-92e5-08b3cf1762d7)

### Context를 사용해서 할 일

- 어떠한 컴포넌트에서 총 가격을 Update 해주는 것
- 어떠한 컴포넌트에서 총 가격을 보여주는 것

### Context 사용

1. Context 생성
    
    **OrderContext**
    
    ```jsx
    import { createContext } from "react";
    
    const OrderContext = createContext();
    ```
    
2. Context를 감쌀 Provider 생성
    
    **index.js**
    
    ```jsx
    import React from 'react';
    import ReactDOM from 'react-dom/client';
    import './index.css';
    import App from './App';
    import reportWebVitals from './reportWebVitals';
    import OrderContext from './context/OrderContext';
    
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(
      <React.StrictMode>
        <OrderContext.Provider value={}>
        <App />
        </OrderContext.Provider>
      </React.StrictMode>
    );
    ```
    
3. 더 복잡한 로직을 구현하기 위해 Provider를 위한 함수 생성
    
    **OrderContext**
    
    ```jsx
    export function OrderContextProvider(props) {
      
      return  <OrderContext.Provider value {...props} />;
    
    }
    ```
    
    **index.js**
    
    ```jsx
    <React.StrictMode>
        <OrderContextProvider>
          <App />
        </OrderContextProvider>
      </React.StrictMode>
    );
    ```
    

### value로 넣을 데이터  만들기

**OrderContext**

```jsx
import { createContext, useMemo, useState } from "react";

const OrderContext = createContext();

export function OrderContextProvider(props) {

  const [orderCounts, setorderCounts] = useState({
    products: new Map(),
    options: new Map()
  })

  const value = useMemo(() => {
    return [{ ...orderCounts }]
  }, [orderCounts])

  return  <OrderContext.Provider value={value} {...props} />;

}
```

## State를 업데이트해주는 함수 작성하기

**OrderContext**

```jsx
import { createContext, useMemo, useState } from "react";

const OrderContext = createContext();

export function OrderContextProvider(props) {

  const [orderCounts, setorderCounts] = useState({
    products: new Map(),
    options: new Map()
  })

  const value = useMemo(() => {

    function updateItemCount(itemName, newItemCount, orderType) {
      // 원래 orderCounts 복사
      const newOrderCounts = { ...orderCounts };
      // prodicts 인지 option인지 잡아준다.
      const orderCountsMap = orderCounts[orderType];
      // 만약 String으로 오면 number로 변경
      orderCountsMap.set(itemName,parseInt(newItemCount));

      setorderCounts(newOrderCounts);
    }

    return [{ ...orderCounts }, updateItemCount]
  }, [orderCounts])

  return  <OrderContext.Provider value={value} {...props} />;

}
```

## 상품 Count를 이용한 가격 계산

상품의 가격을 계산해주는 기능이다.

**OrderContext**

```jsx
import { createContext, useMemo, useState } from "react";

const OrderContext = createContext();

export function OrderContextProvider(props) {

  const [orderCounts, setorderCounts] = useState({
    products: new Map(),
    options: new Map()
  })

  const [totals, settotals] = useState({
    products: 0,
    options: 0,
    total: 0
  })
  
  const pricePerItem = {
    products: 1000,
    options: 500
  }

  const calculateSubtotal = (orderType, orderCounts) => {
    let optionCount = 0;
    for (const count of orderCounts[orderType].values()) {
      optionCount += count;
    }

    return optionCount * pricePerItem[orderType];
  }

  useEffect(() => {
    const productsTotal = calculateSubtotal("prodicts", orderCounts);
    const optionsTotal = calculateSubtotal("options", orderCounts);
    const total = productsTotal + optionsTotal;
    setTotals({
      products: productsTotal,
      options: optionsTotal,
      total
    })
  }, [orderCounts])
  

  const value = useMemo(() => {
    function updateItemCount(itemName, newItemCount, orderType) {
      const newOrderCounts = { ...orderCounts };
      const orderCountsMap = orderCounts[orderType];
      orderCountsMap.set(itemName,parseInt(newItemCount));
      setorderCounts(newOrderCounts);
    }

    return [{ ...orderCounts, totals }, updateItemCount]
  }, [orderCounts, totals])

  return  <OrderContext.Provider value={value} {...props} />;

}
```

### orderContext 사용하기

**OrderContext**

```jsx
export const OrderContext = createContext();
```

**Type**

```jsx
const Type = ({ orderType }) => {
  
  const [items, setItems] = useState([]);
  const [error, setError] = useState(false);
  const [orderData, updateItemCount] = useContext(OrderContext);

  useEffect(() => {
    loadItems(orderType);
  }, [orderType])
```

## 계산된 가격 보여주기

이제 Context를 사용할 준비를 끝냈다. 이 Context를 사용해서 여행상품, 옵션을 추가 함에 따라 여행상품의 총 가격과 옵션의 총 가격 그리고 여행상품과 옵션의 총가격을 더한 전체 총가격을 나타내보자.

### 구현 내용

- 여행 상품의 총 가격, 옵션의 총 가격 구하기
1. 여행 가격은 각 상품의 숫자를 올리거나 내릴 때(Products 컴포넌트)
2. 옵션은 각 옵션의 체크 박스를 체크하거나 제거할 때(Options 컴포넌트)

### 코드

**Type**

```jsx
const optionItems = items.map(item => (
    <ItemComponent
      key={item.name}
      name={item.name}
      imagePath={item.imagePath}
      updateItemCount={(itemName, newItemCount) => updateItemCount(itemName, newItemCount, orderType)}
    />
  ))
```

Products, Options 두 컴포넌트에 updateItemCount Prop을 전달한다.

**Products**

```jsx
import React from 'react'

const Products = ({ name, imagePath, updateItemCount }) => {
  console.log(name, imagePath)

  const handleChange = (event) => {
    const currentValue = event.target.value;
    updateItemCount(name, currentValue);
  }
  return (
    <div style={{ textAlign: 'center' }}>
      <img
        style={{width: '75%'}}
        src={`http://localhost:4000/${imagePath}`}
        alt={`${name} product`}
      />
      <form style={{ marginTop: '10px' }}>
        <label style={{ textAlign: 'right' }}>{name}</label>
        <input
          style={{ marginLeft: '7px' }}
          type="number"
          name="quantity"
          min="0"
          defaultValue={0}
          onChange={handleChange}
        />
      </form>
    </div>
  )
}

export default Products
```

**Options**

```jsx
import React from 'react'

const Options = ({ name, updateItemCount }) => {
  return (
    <form>
      <input type="checkbox" id={`${name} option`} 
        onChange={(e) => updateItemCount(name, e.target.checked ? 1: 0)}
      />{" "}
      <label htmlFor={`${name} option`}>{name}</label>
    </form>
  )
}

export default Options
```

**여행 상품의 총 가격, 옵션의 가격 보여주기**

```jsx
return (
    <div>
      <h2>주문 종류</h2>
      <p>하나의 가격</p>
      <p>총 가격: {orderData.totals[orderType]}</p>
      <div 
          style={{ 
            display: 'flex',
            flexDirection: orderType === "oprions" ? "colum" : "row"
          }}
       >
          {optionItems}
       </div>
    </div>
  )
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
