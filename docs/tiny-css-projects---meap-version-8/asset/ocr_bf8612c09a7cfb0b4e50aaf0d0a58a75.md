```
case 'Enter':
    toggleMenu();
    break;
case '':
    navigate(event);
    toggleMenu();
    break;
case 'Tab':
    event.preventDefault();
    toggleMenu();
    break;
case 'Escape':
    toggleMenu();
    break;
case 'Home':
    moveToNext(0);
    break;
case 'End':
    moveToNext(menuItems.length - 1);
    break;
}
}

function handleTab(event) {
    if (event.key !== 'Tab') { return; }
    event.preventDefault();
}

function toggleMenu(event) {
    expanded = !expanded;
    shareButton.ariaExpanded = expanded;
    container.classList.toggle('share_expanded');
    if (expanded) {
        menuItems.forEach(li => li.removeAttribute('tabindex'));
    }
    if (!expanded) {
        menuItems.forEach(li => {
            li.removeAttribute('data-current');
            li.tabIndex = -1;
        })
        shareButton.focus();
    }
}

function moveToNext(next = undefined) {
    const selectedIndex = menuItems.findIndex(li => li.dataset.current === 'true');

    let newIndex
    if (next) {
        newIndex = next;
    } else if (selectedIndex === -1 || selectedIndex === menuItems.length - 1) {
        newIndex = 0;
    }
    else {
        newIndex = selectedIndex + 1;
    }

    if (selectedIndex !== -1) {
        menuItems[selectedIndex].removeAttribute('data-current');
```