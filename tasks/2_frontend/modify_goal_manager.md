import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSmile } from '@fortawesome/free-solid-svg-icons';
import { AddIconButtonContainer, TransparentButton, AddIconButtonText } from './StyledComponents'; // Adjust based on your project structure

interface Props {
  goal: {
    id: string;
    icon: string | null;
  };
}

export function GoalManager(props: Props) {
  const [icon, setIcon] = useState<string | null>(null);

  useEffect(() => {
    if (props.goal) {
      setIcon(props.goal.icon);
    }
  }, [props.goal.id, props.goal.icon]);

  const hasIcon = icon !== null;

  const addIconOnClick = (event: React.MouseEvent) => {
    event.stopPropagation();
    setEmojiPickerIsOpen(true);
  };

  return (
    <AddIconButtonContainer hasIcon={hasIcon}>
      <TransparentButton 
        onClick={addIconOnClick} 
        aria-label="Add an icon to goal"
      >
        <FontAwesomeIcon icon={faSmile} size="2x" />
        <AddIconButtonText>Add icon</AddIconButtonText>
      </TransparentButton>
    </AddIconButtonContainer>
  );
}
