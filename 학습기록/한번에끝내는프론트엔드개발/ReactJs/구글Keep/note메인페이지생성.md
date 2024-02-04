# Note 메인 페이지 생성하기

![스크린샷 2024-02-04 오후 5 17 34](https://github.com/Heo-y-y/development-blog/assets/112863029/c68b92f8-2a87-4e9a-bf5b-3c0f2eea8bbd)

### AllNotes.tsx

```tsx
import React, { useState } from 'react'
import { useAppDispatch, useAppSelector } from '../../hooks/redux'
import { Container } from '../ErrorPage/ErrorPage.styles';
import { ButtonOutline, EmptyMsgBox } from '../../styles/styles';
import { Box, InputBox, TopBox } from './AllNotes.styles';
import { toggleFiltersModal } from '../../store/modal/modalSlice';

const AllNotes = () => {
  const dispatch = useAppDispatch();
  const { mainNotes } = useAppSelector((state) => state.notesList);
  const [filter, setFilter] = useState('');
  const [searchInput, setSearchInput] = useState(''); 
  return (
    <Container>
      {mainNotes.length === 0 ? (
      <EmptyMsgBox>
        노트가 없습니다.
      </EmptyMsgBox>
      ) : (
        <>
        <TopBox>
          <InputBox>
          <input
            type={"text"}
            value={searchInput}
            onChange={(e) => setSearchInput(e.target.value)}
            placeholder="노트의 제목을 입력해주세요."
          />
          </InputBox>
          <div className='notes__filter-btn'>
            <ButtonOutline
              onClick={() => dispatch(toggleFiltersModal(true))}
              className='nav__btn'
            >
              <span>정렬</span>
            </ButtonOutline>

          </div>
        </TopBox>
        <Box>
          {/* Notes */}

        </Box>
      </>
      )}
    </Container>
  )
}

export default AllNotes
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
