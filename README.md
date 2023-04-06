# assignment1
chat web application where 2 or more users can chat with each other, apart from that the chat should have the following features/ text-format options in the chat text input window
1.Setup Node.js server: You can start by setting up a Node.js server using a framework like Express.js. Install the required dependencies and create routes for handling user authentication and chat messages.

2.Create a user interface: Create a user interface for the chat application using React.js. Use a library like Material-UI to create a responsive and intuitive design for the chat window and input window.

3.Implement real-time communication: Use a library like Socket.IO to enable real-time communication between the users. When a user sends a message, it should be broadcasted to all the users in the chat room.

4.Add text-formatting options: Implement the text-formatting options in the chat input window. You can use a library like Draft.js to handle the text formatting options like bold, italic, strikethrough, hyperlink, bulleted list, numbered list, blockquote, code snippet, code block, and mention someone.

5.Add file upload: Allow users to upload files like images, videos, and documents. You can use a library like React-Dropzone to handle file uploads.

6.Add emoji support: Implement emoji support in the chat application. You can use a library like React-Emoji to handle emojis.

7.Add web link preview: Implement web link preview in the chat application. When a user sends a web link, it should be previewed with a thumbnail, title, and description.

8.Implement message history: Store the chat message history in a database and fetch it when a user logs in to the chat room.

9Secure the application: Implement security measures to protect the chat application from security threats like XSS attacks and SQL injection attacks.

10.Deploy the application: Deploy the chat application on a server like Heroku or AWS to make it accessible to users.




import React from 'react';
import { Editor, EditorState, RichUtils } from 'draft-js';
import 'draft-js/dist/Draft.css';

const ChatInputWindow = () => {
  const [editorState, setEditorState] = React.useState(EditorState.createEmpty());

  const handleKeyCommand = (command, editorState) => {
    const newState = RichUtils.handleKeyCommand(editorState, command);

    if (newState) {
      setEditorState(newState);
      return 'handled';
    }

    return 'not-handled';
  };

  const handleBoldClick = () => {
    setEditorState(RichUtils.toggleInlineStyle(editorState, 'BOLD'));
  };

  const handleItalicClick = () => {
    setEditorState(RichUtils.toggleInlineStyle(editorState, 'ITALIC'));
  };

  const handleStrikethroughClick = () => {
    setEditorState(RichUtils.toggleInlineStyle(editorState, 'STRIKETHROUGH'));
  };

  const handleUnderlineClick = () => {
    setEditorState(RichUtils.toggleInlineStyle(editorState, 'UNDERLINE'));
  };

  const handleLinkClick = () => {
    const selection = editorState.getSelection();
    const link = window.prompt('Enter the URL of the link:');

    if (!link) {
      setEditorState(RichUtils.toggleLink(editorState, selection, null));
      return;
    }

    const content = editorState.getCurrentContent();
    const contentWithEntity = content.createEntity('LINK', 'MUTABLE', { url: link });
    const newEditorState = EditorState.push(editorState, contentWithEntity, 'create-entity');
    const entityKey = contentWithEntity.getLastCreatedEntityKey();
    const newSelection = selection.merge({
      focusOffset: selection.getFocus
