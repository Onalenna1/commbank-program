// GoalCard.tsx
import React from 'react';
import styled from 'styled-components';

const Container = styled.div`
  /* styles for the card */
  cursor: pointer;
`;

const Icon = styled.span`
  font-size: 5.5rem; // Use a span instead of h1 for the icon
`;

interface Props {
  goal: {
    id: string;
    icon: string; // Assuming this could be an image URL or an emoji
    // additional properties for the goal
  };
  onClick: () => void;
}

export default function GoalCard({ goal, onClick }: Props) {
  return (
    <Container key={goal.id} onClick={onClick} role="button" tabIndex={0}>
      {/* Display icon based on type */}
      {goal.icon.startsWith('http') ? (
        <img src={goal.icon} alt="Goal icon" style={{ width: '5.5rem', height: 'auto' }} />
      ) : (
        <Icon>{goal.icon}</Icon>
      )}
    </Container>
  );
}
