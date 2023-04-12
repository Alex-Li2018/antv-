# canvas documnet

为了实现画布的节点之间的层级，所以doucmnet自己实现了类似dom tree的结构。具体dom tree中涉及的类 document node element这三个

## abstract node

abstract class Node extends EventTarget implements INode

**Node 继承了 EventTarget 实现了INode接口**

对应的接口

```js
interface INode extends IEventTarget {
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
```
其中**appendChild** **cloneNode** **removeChild** **replaceChild**等由继承类实现


## documnet

Document -> Node -> EventTarget, dom tree的入口。**class Document extends Node implements IDocument**

```js
interface IParentNode {
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

interface IDocument extends INode, IParentNode {
  /**
   * Returns the Window object of the active document.
   */
  readonly defaultView: ICanvas | null;

  /**
   * Gets a reference to the root node of the document.
   */
  readonly documentElement: IElement;

  readonly ownerDocument: null;
  readonly timeline: IAnimationTimeline;

  /**
   * Creates an instance of the element for the specified tag.
   */
  createElement: <
    T extends DisplayObject<StyleProps>,
    StyleProps extends BaseStyleProps,
  >(
    tagName: string,
    options: DisplayObjectConfig<StyleProps>,
  ) => T;

  elementFromPoint: (x: number, y: number) => Promise<DisplayObject>;
  elementsFromPoint: (x: number, y: number) => Promise<DisplayObject[]>;
  elementsFromBBox: (
    minX: number,
    minY: number,
    maxX: number,
    maxY: number,
  ) => DisplayObject[];
}
```


## Element

```js
class Element<
    StyleProps extends BaseStyleProps = any,
    ParsedStyleProps extends ParsedBaseStyleProps = any,
  >
  extends Node
  implements IElement<StyleProps, ParsedStyleProps>
```

```js
interface IElement<StyleProps = any, ParsedStyleProps = any>
  extends INode,
    IChildNode,
    IParentNode {
  /**
   * Returns the value of element's id content attribute. Can be set to change it.
   */
  id: string;

  /**
   * Returns the qualified name.
   */
  tagName: string;

  name: string;

  /**
   * Returns the value of element's class content attribute. Can be set to change it.
   */
  className: string;
  classList: string[];

  /**
   * @see https://developer.mozilla.org/en-US/docs/Web/API/Element/attributes
   */
  attributes: StyleProps;

  /**
   * compatible with `style`
   * @see https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style
   */
  style: StyleProps & ICSSStyleDeclaration<StyleProps>;
  parsedStyle: ParsedStyleProps;

  getElementById: <E extends IElement = IElement>(id: string) => E | null;
  getElementsByName: <E extends IElement = IElement>(name: string) => E[];
  getElementsByClassName: <E extends IElement = IElement>(
    className: string,
  ) => E[];
  getElementsByTagName: <E extends IElement = IElement>(tagName: string) => E[];

  scrollLeft: number;
  scrollTop: number;
  clientLeft: number;
  clientTop: number;

  getGeometryBounds(): AABB;
  getRenderBounds(): AABB;
  getBounds(): AABB;
  getLocalBounds(): AABB;
  getBoundingClientRect(): Rectangle;
  getClientRects(): Rectangle[];

  /**
   * @see https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute
   */
  setAttribute: <Key extends keyof StyleProps>(
    attributeName: Key,
    value: StyleProps[Key],
    force?: boolean,
  ) => void;

  /**
   * @see https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute
   */
  getAttribute: (
    attributeName: keyof StyleProps,
  ) => StyleProps[keyof StyleProps] | undefined;

  /**
   * @see https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute
   */
  removeAttribute: (attributeName: keyof StyleProps) => void;

  hasAttribute: (qualifiedName: string) => boolean;
}
```