# Sidebar 생성

![스크린샷 2024-02-02 오후 2 56 32](https://github.com/Heo-y-y/development-blog/assets/112863029/5898faca-f50c-4ffb-bea5-42804331cc13)

## UI

### Sidebar.tsx

```tsx
import React from 'react'
import { NavLink, useLocation } from 'react-router-dom'
import { useAppDispatch, useAppSelector } from '../../hooks/redux';
import { Container, MainBox, StyledLogo, ItemsBox } from './Sidebar.styles';
import { toggleMenu } from '../../store/menu/menuSlice';
import { FaArchive, FaLightbulb, FaTag, FaTrash } from 'react-icons/fa';
import getStandardName from '../../utils/getStandardName';
import { toggleTagsModal } from '../../store/modal/modalSlice';
import { MdEdit } from 'react-icons/md';
import { v4 } from 'uuid';

const items = [
  { icon: <FaArchive />, title: "Archive", id: v4() },
  { icon: <FaTrash />, title: "Trash", id: v4() },
]

const Sidebar = () => {
  const dispatch = useAppDispatch();

  const { isOpen } = useAppSelector((state) => state.menu);
  const { tagsList } = useAppSelector((state) => state.tags);

  const { pathname } = useLocation();

  if (pathname === "/404") {
    return null;
  }

  return (
    <Container openMenu={isOpen ? "open" : ""}>
      <MainBox openMenu={isOpen ? "open" : ""}>
        <StyledLogo>
          <h1>Keep</h1>
        </StyledLogo>

        <ItemsBox >
          {/* note item */}
          <li onClick={() => dispatch(toggleMenu(false))}>
            <NavLink
              to={"/"}
              state={`notes`}
              className={({ isActive }) => isActive ? "active-item" : "inactive-item"}
            >
              <span>
                <FaLightbulb />
              </span>
              <span>Notes</span>
            </NavLink>
          </li>

          {/* tag items */}
          {tagsList?.map(({ tag, id }) => (
            <li key={id} onClick={() => dispatch(toggleMenu(false))}>
              <NavLink
                to={`/tag/${tag}`}
                state={`${tag}`}
                className={({ isActive }) => isActive ? "active-item" : "inactive-item"}
              >
                <span>
                  <FaTag />
                </span>
                <span>{getStandardName(tag)}</span>
              </NavLink>
            </li>
          ))}

          {/* edit tag item */}
          <li
            className='sidebar__edit-item'
            onClick={() => dispatch(toggleTagsModal({ type: "edit", view: true }))}
          >
            <span>
              <MdEdit />
            </span>
            <span>Edit Notes</span>
          </li>

          {/* other items */}
          {items.map(({ icon, title, id }) => (
            <li key={id} onClick={() => dispatch(toggleMenu(false))}>
              <NavLink
                to={`/${title.toLocaleLowerCase()}`}
                state={`${title}`}
                className={({ isActive }) => isActive ? "active-item" : "inactive-item"}
              >
                <span>{icon}</span>
                <span>{title}</span>
              </NavLink>
            </li>
          ))}

        </ItemsBox>
      </MainBox>
    </Container>
  )
}

export default Sidebar
```

### tagSlice.ts

```tsx
import { createSlice } from "@reduxjs/toolkit";
import { v4 } from 'uuid';

const initialState = {
  tagsList: [
    { tag: "coding", id: v4() },
    { tag: "exercise", id: v4() },
    { tag: "quotes", id: v4() }
  ]
}

const tagsSlice = createSlice({
  name: "tags",
  initialState,
  reducers: {}
})

export default tagsSlice.reducer;
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
