# Navbar 생성

![스크린샷 2024-02-02 오후 2 02 23](https://github.com/Heo-y-y/development-blog/assets/112863029/ef972969-13d1-4785-9dba-61d330df60ad)

## UI

### Navber.tsx

```tsx
import React from 'react'
import { Container, StyledNav } from './Navbar.styles'
import { FiMenu } from 'react-icons/fi';
import { ButtonFill } from '../../styles/styles';
import { NavLink, useLocation } from 'react-router-dom';
import { useAppDispatch } from '../../hooks/redux';
import { toggleMenu } from '../../store/menu/menuSlice';
import { toggleCreateNoteModal } from '../../store/modal/modalSlice';
import getStandardName from '../../utils/getStandardName';

const Navbar = () => {
  const dispatch = useAppDispatch();

  const { pathname, state } = useLocation()
  console.log(state);
  if (pathname === "/404") {
    return null;
  }

  return (
    <StyledNav>
      <div className='nav__menu'>
        <FiMenu onClick={() => dispatch(toggleMenu(true))} />
      </div>

      <Container>
        <div className='nav__page-title'>{getStandardName(state)} </div>

        {state !== "Trash" && state !== "Archive" &&
          <ButtonFill
            onClick={() => dispatch(toggleCreateNoteModal(true))}
            className="nav__btn"
          >
            <span>+</span>
          </ButtonFill>
        }

      </Container>
    </StyledNav>
  )
}

export default Navbar
```

### menuSlice.ts

```tsx
const menuSlice = createSlice({
  name: "menu",
  initialState,
	// 추가
  reducers: {
    toggleMenu: (state, action) => {
      state.isOpen = action.payload
    }
  }
})

export const { toggleMenu } = menuSlice.actions;
```

### modalSlice.ts

```tsx
import { createSlice } from "@reduxjs/toolkit";

interface ModalState {
    viewEditTagsModal: boolean,
    viewAddTagsModal: boolean,
    viewCreateNoteModal: boolean,
    viewFiltersModal: boolean
}

const initialState: ModalState = {
    viewEditTagsModal: false,
    viewAddTagsModal: false,
    viewCreateNoteModal: false,
    viewFiltersModal: false
}

const modalSlice = createSlice({
    name: 'modal',
    initialState,
    reducers: {
				// 추가
        toggleTagsModal: (state, { payload }) => {
            const { type, view } = payload;

            if (type === "add") {
                state.viewAddTagsModal = view;
            } else {
                state.viewEditTagsModal = view;
            }

        },

				// 추가
        toggleCreateNoteModal: (state, action) => {
            state.viewCreateNoteModal = action.payload;
        },
				// 추가
        toggleFiltersModal: (state, action) => {
            state.viewFiltersModal = action.payload
        },

    }
})

// 추가
export const { toggleTagsModal, toggleCreateNoteModal, toggleFiltersModal } = modalSlice.actions;
export default modalSlice.reducer;
```

### App.tsx

```tsx
function App() {

  return (
      <div className='App'>
       <BrowserRouter>
        <Sidebar />
       <div className='app__container'>
				// 추가
        <Navbar />
        <Routes>
          <Route path='/' element={<AllNotes />} />
          <Route path='/archive' element={<ArchiveNotes />} />
          <Route path='/trash' element={<TrashNotes />} />
          <Route path='/tag/:name' element={<TagNotes />} />
          <Route path='/404' element={<ErrorPage />} />
          <Route path='/*' element={<Navigate to={"/404"} />} />
        </Routes>
       </div>
       </BrowserRouter>
      </div>
  )
}
```

### getStandardName

```tsx
const getStandardName = (name: string) => {
  return (
    name?.slice(0, 1).toUpperCase() + name?.slice(1, name.length).toLocaleLowerCase()
  )
}

export default getStandardName;
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
