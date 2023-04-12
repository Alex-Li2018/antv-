# Node

三种Node接口 INode IParentNode IChildNode, 他们的区别：

```js
export interface INode extends IEventTarget {
  shadow: boolean;
  /**
   * Returns node's node document's document base URL.
   */
  readonly baseURI: string;
  /**
   * Returns the children.
   */
  readonly childNodes: IChildNode[];
  /**
   * Returns the first child.
   */
  readonly firstChild: IChildNode | null;
  /**
   * Returns true if node is connected and false otherwise.
   */
  isConnected: boolean;
  /**
   * Returns the last child.
   */
  readonly lastChild: IChildNode | null;

  /**
   * Returns the next sibling.
   */
  readonly nextSibling: IChildNode | null;
  /**
   * Returns a string appropriate for the type of node.
   */
  readonly nodeName: string;
  /**
   * Returns the type of node.
   */
  readonly nodeType: number;
  nodeValue: string | null;
  /**
   * Returns the node document. Returns null for documents.
   */
  ownerDocument: IDocument | null;
  /**
   * Returns the parent element.
   */
  readonly parentElement: IElement | null;
  /**
   * Returns the parent.
   */
  parentNode: (INode & IParentNode) | null;
  /**
   * Returns the previous sibling.
   */
  readonly previousSibling: IChildNode | null;
  textContent: string | null;
  appendChild: <T extends INode>(newChild: T, index?: number) => T;
  /**
   * Returns a copy of node. If deep is true, the copy also includes the node's descendants.
   */
  cloneNode: (deep?: boolean) => INode;
  /**
   * Returns a bitmask indicating the position of other relative to node.
   */
  compareDocumentPosition: (other: INode) => number;
  /**
   * Returns true if other is an inclusive descendant of node, and false otherwise.
   */
  contains: (other: INode | null) => boolean;
  /**
   * Returns node's root.
   */
  getRootNode: (options?: GetRootNodeOptions) => INode;
  /**
   * Returns node's ancestor.
   */
  getAncestor: (n: number) => INode | null;
  /**
   * Traverse in sub tree.
   */
  forEach: (callback: (o: INode) => void | boolean, assigned?: boolean) => void;
  /**
   * Returns whether node has children.
   */
  hasChildNodes: () => boolean;
  insertBefore: <T extends INode>(newChild: T, refChild: INode | null) => T;
  isDefaultNamespace: (namespace: string | null) => boolean;
  /**
   * Returns whether node and otherNode have the same properties.
   */
  isEqualNode: (otherNode: INode | null) => boolean;
  isSameNode: (otherNode: INode | null) => boolean;
  lookupNamespaceURI: (prefix: string | null) => string | null;
  lookupPrefix: (namespace: string | null) => string | null;
  /**
   * Removes empty exclusive Text nodes and concatenates the data of remaining contiguous exclusive Text nodes into the first of their nodes.
   */
  normalize: () => void;
  removeChild: <T extends INode>(oldChild: T) => T;
  replaceChild: <T extends INode>(newChild: INode, oldChild: T) => T;

  /**
   * Destroy itself.
   */
  destroy: () => void;
}

export interface IParentNode {
  readonly childElementCount: number;
  /**
   * Returns the child elements.
   */
  readonly children: IElement[];
  /**
   * Returns the first child that is an element, and null otherwise.
   */
  readonly firstElementChild: IElement | null;
  /**
   * Returns the last child that is an element, and null otherwise.
   */
  readonly lastElementChild: IElement | null;
  /**
   * Inserts nodes after the last child of node, while replacing strings in nodes with equivalent Text nodes.
   *
   * Throws a "HierarchyRequestError" DOMException if the constraints of the node tree are violated.
   */
  append: (...nodes: INode[]) => void;
  /**
   * Inserts nodes before the first child of node, while replacing strings in nodes with equivalent Text nodes.
   *
   * Throws a "HierarchyRequestError" DOMException if the constraints of the node tree are violated.
   */
  prepend: (...nodes: INode[]) => void;
  /**
   * Returns the first element that is a descendant of node that matches selectors.
   */
  querySelector: <E extends IElement = IElement>(selectors: string) => E | null;
  /**
   * Returns all element descendants of node that match selectors.
   */
  querySelectorAll: <E extends IElement = IElement>(selectors: string) => E[];
  /**
   * Similar to querySelector, use custom filter instead of selectors.
   */
  find: <E extends IElement = IElement>(
    filter: (node: E) => boolean,
  ) => E | null;
  /**
   * Similar to querySelectorAll, use custom filter instead of selectors.
   */
  findAll: <E extends IElement = IElement>(filter: (node: E) => boolean) => E[];
}

export interface IChildNode extends INode {
  /**
   * Inserts nodes just after node, while replacing strings in nodes with equivalent Text nodes.
   *
   * Throws a "HierarchyRequestError" DOMException if the constraints of the node tree are violated.
   */
  after: (...nodes: INode[]) => void;
  /**
   * Inserts nodes just before node, while replacing strings in nodes with equivalent Text nodes.
   *
   * Throws a "HierarchyRequestError" DOMException if the constraints of the node tree are violated.
   */
  before: (...nodes: INode[]) => void;
  /**
   * Removes node.
   */
  remove: () => void;
  /**
   * Replaces node with nodes, while replacing strings in nodes with equivalent Text nodes.
   *
   * Throws a "HierarchyRequestError" DOMException if the constraints of the node tree are violated.
   */
  replaceWith: (...nodes: INode[]) => void;
}
```
