### 구현 화면

![스크린샷 2024-01-22 오후 2 55 28](https://github.com/Heo-y-y/development-blog/assets/112863029/9d01ad2f-2866-4338-ad7e-b7a2a3699256)

### 구현 내용

- 주문 확인 체크 박스를 눌러야만 주문 확인 버튼을 누를 수 있다.

### 코드

```jsx
import React, { useState } from 'react'

const SummaryPage = () => {
  const [checked, setChecked] = useState(false);
  return (
    <div>
      <form>
        <input 
          type="checkbox"
          checked={checked}
          id="confirm-checkbox"
          onChange={(e) => setChecked(e.target.checked)}
        />{" "}
        <label htmlFor='confirm-checkbox'>
          주문하려는 것을 확인하셨나요?
        </label>
        <br />
        <button disabled={!checked} type='submit'>
          주문 확인
        </button>
      </form>
    </div>
  )
}

export default SummaryPage
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
